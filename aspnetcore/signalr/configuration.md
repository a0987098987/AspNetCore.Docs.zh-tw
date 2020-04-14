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
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="7592c-103">ASP.NET Core SignalR 組態</span><span class="sxs-lookup"><span data-stu-id="7592c-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.1"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="7592c-104">JSON/訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="7592c-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="7592c-105">ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="7592c-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="7592c-106">每個協定都有序列化配置選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="7592c-107">JSON 序列化可以使用[AddJson 協定](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="7592c-108">`AddJsonProtocol`可以在`Startup.ConfigureServices`[中新增SignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)後新增 。</span><span class="sxs-lookup"><span data-stu-id="7592c-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7592c-109">該方法`AddJsonProtocol`採用`options`接收 物件的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="7592c-110">該物件的[Payload 序列化器選項](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是`System.Text.Json`<xref:System.Text.Json.JsonSerializerOptions>可用於配置參數和返回值序列化的物件。</span><span class="sxs-lookup"><span data-stu-id="7592c-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="7592c-111">關於詳細資訊,請參閱[系統.Text.Json 文件](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="7592c-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="7592c-112">例如,要將序列化程式設定為不變更屬性名稱的大小寫,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:</span><span class="sxs-lookup"><span data-stu-id="7592c-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="7592c-113">在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="7592c-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="7592c-114">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:</span><span class="sxs-lookup"><span data-stu-id="7592c-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="7592c-115">此時無法在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="7592c-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="7592c-116">切換到牛頓軟. Json</span><span class="sxs-lookup"><span data-stu-id="7592c-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="7592c-117">如果您需要在 中不`Newtonsoft.Json`支援`System.Text.Json`的功能 ,請參閱[切換到 Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson)。</span><span class="sxs-lookup"><span data-stu-id="7592c-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="7592c-118">訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="7592c-118">MessagePack serialization options</span></span>

<span data-ttu-id="7592c-119">消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="7592c-120">有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。</span><span class="sxs-lookup"><span data-stu-id="7592c-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="7592c-121">此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="7592c-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="7592c-122">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="7592c-122">Configure server options</span></span>

<span data-ttu-id="7592c-123">下表描述了用於設定 SignalR 集線器的選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="7592c-124">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-124">Option</span></span> | <span data-ttu-id="7592c-125">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-125">Default Value</span></span> | <span data-ttu-id="7592c-126">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="7592c-127">30 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-127">30 seconds</span></span> | <span data-ttu-id="7592c-128">如果用戶端未在此間隔內收到消息(包括保持活動狀態),伺服器將考慮用戶端斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="7592c-129">由於如何實現,實際將客戶端標記為斷開連接的時間可能比此超時間隔長。</span><span class="sxs-lookup"><span data-stu-id="7592c-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="7592c-130">建議的值是值的`KeepAliveInterval`兩倍。</span><span class="sxs-lookup"><span data-stu-id="7592c-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="7592c-131">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-131">15 seconds</span></span> | <span data-ttu-id="7592c-132">如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。</span><span class="sxs-lookup"><span data-stu-id="7592c-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="7592c-133">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-134">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7592c-135">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-135">15 seconds</span></span> | <span data-ttu-id="7592c-136">如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。</span><span class="sxs-lookup"><span data-stu-id="7592c-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="7592c-137">更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="7592c-138">建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /</span><span class="sxs-lookup"><span data-stu-id="7592c-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="7592c-139">所有已安裝的協定</span><span class="sxs-lookup"><span data-stu-id="7592c-139">All installed protocols</span></span> | <span data-ttu-id="7592c-140">此中心支持的協定。</span><span class="sxs-lookup"><span data-stu-id="7592c-140">Protocols supported by this hub.</span></span> <span data-ttu-id="7592c-141">默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。</span><span class="sxs-lookup"><span data-stu-id="7592c-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="7592c-142">如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。</span><span class="sxs-lookup"><span data-stu-id="7592c-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="7592c-143">默認值為`false`,因為這些異常消息可能包含敏感資訊。</span><span class="sxs-lookup"><span data-stu-id="7592c-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="7592c-144">可緩衝用戶端上載流的最大項數。</span><span class="sxs-lookup"><span data-stu-id="7592c-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="7592c-145">如果達到此限制,則在伺服器處理流項之前將阻止調用的處理。</span><span class="sxs-lookup"><span data-stu-id="7592c-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="7592c-146">32 KB</span><span class="sxs-lookup"><span data-stu-id="7592c-146">32 KB</span></span> | <span data-ttu-id="7592c-147">單個傳入中心消息的最大大小。</span><span class="sxs-lookup"><span data-stu-id="7592c-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="7592c-148">可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="7592c-149">單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :</span><span class="sxs-lookup"><span data-stu-id="7592c-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="7592c-150">進階 HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="7592c-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="7592c-151">用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="7592c-152">這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub)在`Startup.Configure`中配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="7592c-153">下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="7592c-154">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-154">Option</span></span> | <span data-ttu-id="7592c-155">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-155">Default Value</span></span> | <span data-ttu-id="7592c-156">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="7592c-157">32 KB</span><span class="sxs-lookup"><span data-stu-id="7592c-157">32 KB</span></span> | <span data-ttu-id="7592c-158">在應用背壓之前,伺服器緩衝的用戶端接收的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="7592c-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="7592c-159">增加此值允許伺服器在不施加背壓的情況下更快地接收更大的消息,但會增加記憶體消耗。</span><span class="sxs-lookup"><span data-stu-id="7592c-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="7592c-160">數據自動從應用於中心`Authorize`類的屬性中收集。</span><span class="sxs-lookup"><span data-stu-id="7592c-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="7592c-161">用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。</span><span class="sxs-lookup"><span data-stu-id="7592c-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="7592c-162">32 KB</span><span class="sxs-lookup"><span data-stu-id="7592c-162">32 KB</span></span> | <span data-ttu-id="7592c-163">在觀察背壓之前,伺服器緩衝的應用發送的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="7592c-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="7592c-164">增加此值允許伺服器更快地緩衝較大的郵件,而無需等待回壓,但會增加記憶體消耗。</span><span class="sxs-lookup"><span data-stu-id="7592c-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="7592c-165">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="7592c-165">All Transports are enabled.</span></span> | <span data-ttu-id="7592c-166">位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="7592c-167">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="7592c-167">See below.</span></span> | <span data-ttu-id="7592c-168">特定於長輪詢傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="7592c-169">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="7592c-169">See below.</span></span> | <span data-ttu-id="7592c-170">特定於 WebSocket 傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-170">Additional options specific to the WebSockets transport.</span></span> |
| `MinimumProtocolVersion` | <span data-ttu-id="7592c-171">0</span><span class="sxs-lookup"><span data-stu-id="7592c-171">0</span></span> | <span data-ttu-id="7592c-172">指定協商協定的最低版本。</span><span class="sxs-lookup"><span data-stu-id="7592c-172">Specify the minimum version of the negotiate protocol.</span></span> <span data-ttu-id="7592c-173">這用於將用戶端限制為較新版本。</span><span class="sxs-lookup"><span data-stu-id="7592c-173">This is used to limit clients to newer versions.</span></span> |

<span data-ttu-id="7592c-174">長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-174">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="7592c-175">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-175">Option</span></span> | <span data-ttu-id="7592c-176">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-176">Default Value</span></span> | <span data-ttu-id="7592c-177">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-177">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="7592c-178">90 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-178">90 seconds</span></span> | <span data-ttu-id="7592c-179">伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。</span><span class="sxs-lookup"><span data-stu-id="7592c-179">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="7592c-180">減小此值會導致用戶端更頻繁地發出新的輪詢請求。</span><span class="sxs-lookup"><span data-stu-id="7592c-180">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="7592c-181">WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-181">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="7592c-182">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-182">Option</span></span> | <span data-ttu-id="7592c-183">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-183">Default Value</span></span> | <span data-ttu-id="7592c-184">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-184">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="7592c-185">5 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-185">5 seconds</span></span> | <span data-ttu-id="7592c-186">伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。</span><span class="sxs-lookup"><span data-stu-id="7592c-186">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="7592c-187">可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-187">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="7592c-188">委託接收用戶端請求的值作為輸入,並預期返回所需的值。</span><span class="sxs-lookup"><span data-stu-id="7592c-188">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="7592c-189">設定客戶端選項</span><span class="sxs-lookup"><span data-stu-id="7592c-189">Configure client options</span></span>

<span data-ttu-id="7592c-190">可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。</span><span class="sxs-lookup"><span data-stu-id="7592c-190">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="7592c-191">它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。</span><span class="sxs-lookup"><span data-stu-id="7592c-191">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="7592c-192">設定記錄</span><span class="sxs-lookup"><span data-stu-id="7592c-192">Configure logging</span></span>

<span data-ttu-id="7592c-193">使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="7592c-193">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="7592c-194">日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。</span><span class="sxs-lookup"><span data-stu-id="7592c-194">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="7592c-195">有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。</span><span class="sxs-lookup"><span data-stu-id="7592c-195">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="7592c-196">要註冊日誌記錄提供程式,必須安裝必要的包。</span><span class="sxs-lookup"><span data-stu-id="7592c-196">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="7592c-197">有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。</span><span class="sxs-lookup"><span data-stu-id="7592c-197">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="7592c-198">例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="7592c-198">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="7592c-199">呼叫`AddConsole`式副用:</span><span class="sxs-lookup"><span data-stu-id="7592c-199">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="7592c-200">在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。</span><span class="sxs-lookup"><span data-stu-id="7592c-200">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="7592c-201">提供指示`LogLevel`要生產的日誌消息的最小級別的值。</span><span class="sxs-lookup"><span data-stu-id="7592c-201">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="7592c-202">日誌將寫入瀏覽器控制台視窗。</span><span class="sxs-lookup"><span data-stu-id="7592c-202">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="7592c-203">還可以提供表示`LogLevel`日誌級別`string`名稱的值,而不是值。</span><span class="sxs-lookup"><span data-stu-id="7592c-203">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="7592c-204">這在無法存`LogLevel`取 常量的環境中配置 SignalR 日誌記錄時非常有用。</span><span class="sxs-lookup"><span data-stu-id="7592c-204">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="7592c-205">下表列出了可用的日誌級別。</span><span class="sxs-lookup"><span data-stu-id="7592c-205">The following table lists the available log levels.</span></span> <span data-ttu-id="7592c-206">您提供的值來`configureLogging`設置將記錄的**最小**紀錄級別。</span><span class="sxs-lookup"><span data-stu-id="7592c-206">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="7592c-207">將紀錄在此等級紀錄的訊息,**或表中列出的訊息等級**。</span><span class="sxs-lookup"><span data-stu-id="7592c-207">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="7592c-208">String</span><span class="sxs-lookup"><span data-stu-id="7592c-208">String</span></span>                      | <span data-ttu-id="7592c-209">LogLevel</span><span class="sxs-lookup"><span data-stu-id="7592c-209">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="7592c-210">`info`**或**`information`</span><span class="sxs-lookup"><span data-stu-id="7592c-210">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="7592c-211">`warn`**或**`warning`</span><span class="sxs-lookup"><span data-stu-id="7592c-211">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="7592c-212">要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。</span><span class="sxs-lookup"><span data-stu-id="7592c-212">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="7592c-213">關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="7592c-213">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="7592c-214">SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="7592c-214">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="7592c-215">它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。</span><span class="sxs-lookup"><span data-stu-id="7592c-215">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="7592c-216">以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。</span><span class="sxs-lookup"><span data-stu-id="7592c-216">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="7592c-217">如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:</span><span class="sxs-lookup"><span data-stu-id="7592c-217">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="7592c-218">這可以安全地忽略。</span><span class="sxs-lookup"><span data-stu-id="7592c-218">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="7592c-219">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="7592c-219">Configure allowed transports</span></span>

<span data-ttu-id="7592c-220">SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-220">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="7592c-221">值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-221">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="7592c-222">默認情況下,所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="7592c-222">All transports are enabled by default.</span></span>

<span data-ttu-id="7592c-223">例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:</span><span class="sxs-lookup"><span data-stu-id="7592c-223">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="7592c-224">在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:</span><span class="sxs-lookup"><span data-stu-id="7592c-224">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="7592c-225">在此版本中,JAva 用戶端 Websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-225">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="7592c-226">在 Java 用戶端中,`withTransport`使用上的方法選擇`HttpHubConnectionBuilder`傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-226">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="7592c-227">Java 用戶端預設使用 WebSocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-227">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="7592c-228">SignalR Java 用戶端還不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="7592c-228">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="7592c-229">設定無記名身份驗證</span><span class="sxs-lookup"><span data-stu-id="7592c-229">Configure bearer authentication</span></span>

<span data-ttu-id="7592c-230">要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-230">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="7592c-231">在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。</span><span class="sxs-lookup"><span data-stu-id="7592c-231">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="7592c-232">在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-232">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="7592c-233">在這些情況下,訪問權杖作為查詢字串值`access_token`提供。</span><span class="sxs-lookup"><span data-stu-id="7592c-233">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="7592c-234">在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-234">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="7592c-235">在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="7592c-235">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="7592c-236">在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。</span><span class="sxs-lookup"><span data-stu-id="7592c-236">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="7592c-237">[與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html)</span><span class="sxs-lookup"><span data-stu-id="7592c-237">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="7592c-238">調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="7592c-238">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="7592c-239">設定逾時與保持作用選項</span><span class="sxs-lookup"><span data-stu-id="7592c-239">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="7592c-240">`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:</span><span class="sxs-lookup"><span data-stu-id="7592c-240">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="7592c-241">.NET</span><span class="sxs-lookup"><span data-stu-id="7592c-241">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7592c-242">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-242">Option</span></span> | <span data-ttu-id="7592c-243">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-243">Default value</span></span> | <span data-ttu-id="7592c-244">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-244">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="7592c-245">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-245">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-246">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-246">Timeout for server activity.</span></span> <span data-ttu-id="7592c-247">如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-247">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7592c-248">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-248">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-249">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-249">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="7592c-250">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-250">15 seconds</span></span> | <span data-ttu-id="7592c-251">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-251">Timeout for initial server handshake.</span></span> <span data-ttu-id="7592c-252">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-252">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7592c-253">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-253">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-254">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-254">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7592c-255">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-255">15 seconds</span></span> | <span data-ttu-id="7592c-256">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="7592c-256">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7592c-257">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="7592c-257">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7592c-258">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-258">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="7592c-259">在 .NET 用戶端中,超時值`TimeSpan`指定為值。</span><span class="sxs-lookup"><span data-stu-id="7592c-259">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="7592c-260">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7592c-260">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7592c-261">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-261">Option</span></span> | <span data-ttu-id="7592c-262">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-262">Default value</span></span> | <span data-ttu-id="7592c-263">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-263">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="7592c-264">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-264">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-265">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-265">Timeout for server activity.</span></span> <span data-ttu-id="7592c-266">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-266">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="7592c-267">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-267">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-268">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-268">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="7592c-269">15 秒(15,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-269">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="7592c-270">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="7592c-270">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7592c-271">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="7592c-271">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7592c-272">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-272">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="7592c-273">Java</span><span class="sxs-lookup"><span data-stu-id="7592c-273">Java</span></span>](#tab/java)

| <span data-ttu-id="7592c-274">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-274">Option</span></span> | <span data-ttu-id="7592c-275">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-275">Default value</span></span> | <span data-ttu-id="7592c-276">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-276">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="7592c-277">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="7592c-277">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="7592c-278">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-278">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-279">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-279">Timeout for server activity.</span></span> <span data-ttu-id="7592c-280">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-280">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="7592c-281">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-281">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-282">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-282">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="7592c-283">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-283">15 seconds</span></span> | <span data-ttu-id="7592c-284">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-284">Timeout for initial server handshake.</span></span> <span data-ttu-id="7592c-285">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-285">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="7592c-286">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-286">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-287">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-287">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="7592c-288">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="7592c-288">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="7592c-289">15 秒(15,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-289">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="7592c-290">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="7592c-290">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7592c-291">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="7592c-291">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7592c-292">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-292">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="7592c-293">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="7592c-293">Configure additional options</span></span>

<span data-ttu-id="7592c-294">其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:</span><span class="sxs-lookup"><span data-stu-id="7592c-294">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="7592c-295">.NET</span><span class="sxs-lookup"><span data-stu-id="7592c-295">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7592c-296">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-296">.NET Option</span></span> |  <span data-ttu-id="7592c-297">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-297">Default value</span></span> | <span data-ttu-id="7592c-298">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-298">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="7592c-299">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-299">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="7592c-300">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-300">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-301">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-301">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-302">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-302">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="7592c-303">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-303">Empty</span></span> | <span data-ttu-id="7592c-304">要發送到身份驗證請求的 TLS 證書的集合。</span><span class="sxs-lookup"><span data-stu-id="7592c-304">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="7592c-305">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-305">Empty</span></span> | <span data-ttu-id="7592c-306">要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="7592c-306">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="7592c-307">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-307">Empty</span></span> | <span data-ttu-id="7592c-308">要隨每個 HTTP 請求一起發送的認證。</span><span class="sxs-lookup"><span data-stu-id="7592c-308">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="7592c-309">5 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-309">5 seconds</span></span> | <span data-ttu-id="7592c-310">僅限 Web 插座。</span><span class="sxs-lookup"><span data-stu-id="7592c-310">WebSockets only.</span></span> <span data-ttu-id="7592c-311">用戶端關閉後等待伺服器確認關閉請求的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="7592c-311">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="7592c-312">如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-312">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="7592c-313">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-313">Empty</span></span> | <span data-ttu-id="7592c-314">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="7592c-314">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="7592c-315">可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-315">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="7592c-316">不用於 Web 套接字連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-316">Not used for WebSocket connections.</span></span> <span data-ttu-id="7592c-317">此委託必須返回一個非空值,並且它將作為參數接收預設值。</span><span class="sxs-lookup"><span data-stu-id="7592c-317">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="7592c-318">修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。</span><span class="sxs-lookup"><span data-stu-id="7592c-318">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="7592c-319">**替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。**</span><span class="sxs-lookup"><span data-stu-id="7592c-319">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="7592c-320">傳送 HTTP 請求時要使用的 HTTP 代理。</span><span class="sxs-lookup"><span data-stu-id="7592c-320">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="7592c-321">設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="7592c-321">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="7592c-322">這支援使用 Windows 身份驗證。</span><span class="sxs-lookup"><span data-stu-id="7592c-322">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="7592c-323">可用於配置其他 WebSocket 選項的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-323">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="7592c-324">接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。</span><span class="sxs-lookup"><span data-stu-id="7592c-324">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="7592c-325">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7592c-325">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7592c-326">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-326">JavaScript Option</span></span> | <span data-ttu-id="7592c-327">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-327">Default Value</span></span> | <span data-ttu-id="7592c-328">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-328">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="7592c-329">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-329">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="7592c-330">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-330">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-331">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-331">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-332">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-332">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="7592c-333">Java</span><span class="sxs-lookup"><span data-stu-id="7592c-333">Java</span></span>](#tab/java)

| <span data-ttu-id="7592c-334">Java 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-334">Java Option</span></span> | <span data-ttu-id="7592c-335">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-335">Default Value</span></span> | <span data-ttu-id="7592c-336">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-336">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="7592c-337">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-337">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="7592c-338">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-338">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-339">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-339">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-340">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-340">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="7592c-341">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="7592c-341">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="7592c-342">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-342">Empty</span></span> | <span data-ttu-id="7592c-343">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="7592c-343">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="7592c-344">在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:</span><span class="sxs-lookup"><span data-stu-id="7592c-344">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="7592c-345">在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:</span><span class="sxs-lookup"><span data-stu-id="7592c-345">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="7592c-346">在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="7592c-346">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="7592c-347">其他資源</span><span class="sxs-lookup"><span data-stu-id="7592c-347">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="7592c-348">JSON/訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="7592c-348">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="7592c-349">ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="7592c-349">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="7592c-350">每個協定都有序列化配置選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-350">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="7592c-351">JSON 序列化可以使用[AddJson 協定](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-351">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="7592c-352">`AddJsonProtocol`可以在`Startup.ConfigureServices`[中新增SignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)後新增 。</span><span class="sxs-lookup"><span data-stu-id="7592c-352">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7592c-353">該方法`AddJsonProtocol`採用`options`接收 物件的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-353">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="7592c-354">該物件的[Payload 序列化器選項](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是`System.Text.Json`<xref:System.Text.Json.JsonSerializerOptions>可用於配置參數和返回值序列化的物件。</span><span class="sxs-lookup"><span data-stu-id="7592c-354">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="7592c-355">關於詳細資訊,請參閱[系統.Text.Json 文件](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="7592c-355">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="7592c-356">例如,要將序列化程式設定為不變更屬性名稱的大小寫,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:</span><span class="sxs-lookup"><span data-stu-id="7592c-356">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    });
```

<span data-ttu-id="7592c-357">在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="7592c-357">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="7592c-358">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:</span><span class="sxs-lookup"><span data-stu-id="7592c-358">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="7592c-359">此時無法在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="7592c-359">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="7592c-360">切換到牛頓軟. Json</span><span class="sxs-lookup"><span data-stu-id="7592c-360">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="7592c-361">如果您需要在 中不`Newtonsoft.Json`支援`System.Text.Json`的功能 ,請參閱[切換到 Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson)。</span><span class="sxs-lookup"><span data-stu-id="7592c-361">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="7592c-362">訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="7592c-362">MessagePack serialization options</span></span>

<span data-ttu-id="7592c-363">消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-363">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="7592c-364">有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。</span><span class="sxs-lookup"><span data-stu-id="7592c-364">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="7592c-365">此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="7592c-365">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="7592c-366">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="7592c-366">Configure server options</span></span>

<span data-ttu-id="7592c-367">下表描述了用於設定 SignalR 集線器的選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-367">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="7592c-368">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-368">Option</span></span> | <span data-ttu-id="7592c-369">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-369">Default Value</span></span> | <span data-ttu-id="7592c-370">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-370">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="7592c-371">30 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-371">30 seconds</span></span> | <span data-ttu-id="7592c-372">如果用戶端未在此間隔內收到消息(包括保持活動狀態),伺服器將考慮用戶端斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-372">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="7592c-373">由於如何實現,實際將客戶端標記為斷開連接的時間可能比此超時間隔長。</span><span class="sxs-lookup"><span data-stu-id="7592c-373">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="7592c-374">建議的值是值的`KeepAliveInterval`兩倍。</span><span class="sxs-lookup"><span data-stu-id="7592c-374">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="7592c-375">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-375">15 seconds</span></span> | <span data-ttu-id="7592c-376">如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。</span><span class="sxs-lookup"><span data-stu-id="7592c-376">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="7592c-377">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-377">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-378">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-378">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7592c-379">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-379">15 seconds</span></span> | <span data-ttu-id="7592c-380">如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。</span><span class="sxs-lookup"><span data-stu-id="7592c-380">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="7592c-381">更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-381">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="7592c-382">建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /</span><span class="sxs-lookup"><span data-stu-id="7592c-382">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="7592c-383">所有已安裝的協定</span><span class="sxs-lookup"><span data-stu-id="7592c-383">All installed protocols</span></span> | <span data-ttu-id="7592c-384">此中心支持的協定。</span><span class="sxs-lookup"><span data-stu-id="7592c-384">Protocols supported by this hub.</span></span> <span data-ttu-id="7592c-385">默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。</span><span class="sxs-lookup"><span data-stu-id="7592c-385">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="7592c-386">如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。</span><span class="sxs-lookup"><span data-stu-id="7592c-386">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="7592c-387">默認值為`false`,因為這些異常消息可能包含敏感資訊。</span><span class="sxs-lookup"><span data-stu-id="7592c-387">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="7592c-388">可緩衝用戶端上載流的最大項數。</span><span class="sxs-lookup"><span data-stu-id="7592c-388">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="7592c-389">如果達到此限制,則在伺服器處理流項之前將阻止調用的處理。</span><span class="sxs-lookup"><span data-stu-id="7592c-389">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="7592c-390">32 KB</span><span class="sxs-lookup"><span data-stu-id="7592c-390">32 KB</span></span> | <span data-ttu-id="7592c-391">單個傳入中心消息的最大大小。</span><span class="sxs-lookup"><span data-stu-id="7592c-391">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="7592c-392">可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-392">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="7592c-393">單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :</span><span class="sxs-lookup"><span data-stu-id="7592c-393">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="7592c-394">進階 HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="7592c-394">Advanced HTTP configuration options</span></span>

<span data-ttu-id="7592c-395">用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-395">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="7592c-396">這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub)在`Startup.Configure`中配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-396">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="7592c-397">下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-397">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="7592c-398">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-398">Option</span></span> | <span data-ttu-id="7592c-399">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-399">Default Value</span></span> | <span data-ttu-id="7592c-400">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-400">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="7592c-401">32 KB</span><span class="sxs-lookup"><span data-stu-id="7592c-401">32 KB</span></span> | <span data-ttu-id="7592c-402">在應用背壓之前,伺服器緩衝的用戶端接收的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="7592c-402">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="7592c-403">增加此值允許伺服器在不施加背壓的情況下更快地接收更大的消息,但會增加記憶體消耗。</span><span class="sxs-lookup"><span data-stu-id="7592c-403">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="7592c-404">數據自動從應用於中心`Authorize`類的屬性中收集。</span><span class="sxs-lookup"><span data-stu-id="7592c-404">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="7592c-405">用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。</span><span class="sxs-lookup"><span data-stu-id="7592c-405">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="7592c-406">32 KB</span><span class="sxs-lookup"><span data-stu-id="7592c-406">32 KB</span></span> | <span data-ttu-id="7592c-407">在觀察背壓之前,伺服器緩衝的應用發送的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="7592c-407">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="7592c-408">增加此值允許伺服器更快地緩衝較大的郵件,而無需等待回壓,但會增加記憶體消耗。</span><span class="sxs-lookup"><span data-stu-id="7592c-408">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="7592c-409">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="7592c-409">All Transports are enabled.</span></span> | <span data-ttu-id="7592c-410">位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-410">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="7592c-411">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="7592c-411">See below.</span></span> | <span data-ttu-id="7592c-412">特定於長輪詢傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-412">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="7592c-413">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="7592c-413">See below.</span></span> | <span data-ttu-id="7592c-414">特定於 WebSocket 傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-414">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="7592c-415">長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-415">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="7592c-416">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-416">Option</span></span> | <span data-ttu-id="7592c-417">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-417">Default Value</span></span> | <span data-ttu-id="7592c-418">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-418">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="7592c-419">90 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-419">90 seconds</span></span> | <span data-ttu-id="7592c-420">伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。</span><span class="sxs-lookup"><span data-stu-id="7592c-420">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="7592c-421">減小此值會導致用戶端更頻繁地發出新的輪詢請求。</span><span class="sxs-lookup"><span data-stu-id="7592c-421">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="7592c-422">WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-422">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="7592c-423">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-423">Option</span></span> | <span data-ttu-id="7592c-424">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-424">Default Value</span></span> | <span data-ttu-id="7592c-425">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-425">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="7592c-426">5 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-426">5 seconds</span></span> | <span data-ttu-id="7592c-427">伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。</span><span class="sxs-lookup"><span data-stu-id="7592c-427">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="7592c-428">可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-428">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="7592c-429">委託接收用戶端請求的值作為輸入,並預期返回所需的值。</span><span class="sxs-lookup"><span data-stu-id="7592c-429">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="7592c-430">設定客戶端選項</span><span class="sxs-lookup"><span data-stu-id="7592c-430">Configure client options</span></span>

<span data-ttu-id="7592c-431">可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。</span><span class="sxs-lookup"><span data-stu-id="7592c-431">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="7592c-432">它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。</span><span class="sxs-lookup"><span data-stu-id="7592c-432">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="7592c-433">設定記錄</span><span class="sxs-lookup"><span data-stu-id="7592c-433">Configure logging</span></span>

<span data-ttu-id="7592c-434">使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="7592c-434">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="7592c-435">日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。</span><span class="sxs-lookup"><span data-stu-id="7592c-435">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="7592c-436">有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。</span><span class="sxs-lookup"><span data-stu-id="7592c-436">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="7592c-437">要註冊日誌記錄提供程式,必須安裝必要的包。</span><span class="sxs-lookup"><span data-stu-id="7592c-437">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="7592c-438">有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。</span><span class="sxs-lookup"><span data-stu-id="7592c-438">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="7592c-439">例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="7592c-439">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="7592c-440">呼叫`AddConsole`式副用:</span><span class="sxs-lookup"><span data-stu-id="7592c-440">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="7592c-441">在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。</span><span class="sxs-lookup"><span data-stu-id="7592c-441">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="7592c-442">提供指示`LogLevel`要生產的日誌消息的最小級別的值。</span><span class="sxs-lookup"><span data-stu-id="7592c-442">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="7592c-443">日誌將寫入瀏覽器控制台視窗。</span><span class="sxs-lookup"><span data-stu-id="7592c-443">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="7592c-444">還可以提供表示`LogLevel`日誌級別`string`名稱的值,而不是值。</span><span class="sxs-lookup"><span data-stu-id="7592c-444">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="7592c-445">這在無法存`LogLevel`取 常量的環境中配置 SignalR 日誌記錄時非常有用。</span><span class="sxs-lookup"><span data-stu-id="7592c-445">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="7592c-446">下表列出了可用的日誌級別。</span><span class="sxs-lookup"><span data-stu-id="7592c-446">The following table lists the available log levels.</span></span> <span data-ttu-id="7592c-447">您提供的值來`configureLogging`設置將記錄的**最小**紀錄級別。</span><span class="sxs-lookup"><span data-stu-id="7592c-447">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="7592c-448">將紀錄在此等級紀錄的訊息,**或表中列出的訊息等級**。</span><span class="sxs-lookup"><span data-stu-id="7592c-448">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="7592c-449">String</span><span class="sxs-lookup"><span data-stu-id="7592c-449">String</span></span>                      | <span data-ttu-id="7592c-450">LogLevel</span><span class="sxs-lookup"><span data-stu-id="7592c-450">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="7592c-451">`info`**或**`information`</span><span class="sxs-lookup"><span data-stu-id="7592c-451">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="7592c-452">`warn`**或**`warning`</span><span class="sxs-lookup"><span data-stu-id="7592c-452">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="7592c-453">要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。</span><span class="sxs-lookup"><span data-stu-id="7592c-453">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="7592c-454">關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="7592c-454">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="7592c-455">SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="7592c-455">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="7592c-456">它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。</span><span class="sxs-lookup"><span data-stu-id="7592c-456">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="7592c-457">以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。</span><span class="sxs-lookup"><span data-stu-id="7592c-457">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="7592c-458">如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:</span><span class="sxs-lookup"><span data-stu-id="7592c-458">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="7592c-459">這可以安全地忽略。</span><span class="sxs-lookup"><span data-stu-id="7592c-459">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="7592c-460">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="7592c-460">Configure allowed transports</span></span>

<span data-ttu-id="7592c-461">SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-461">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="7592c-462">值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-462">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="7592c-463">默認情況下,所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="7592c-463">All transports are enabled by default.</span></span>

<span data-ttu-id="7592c-464">例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:</span><span class="sxs-lookup"><span data-stu-id="7592c-464">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="7592c-465">在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:</span><span class="sxs-lookup"><span data-stu-id="7592c-465">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="7592c-466">在此版本中,JAva 用戶端 Websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-466">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="7592c-467">在 Java 用戶端中,`withTransport`使用上的方法選擇`HttpHubConnectionBuilder`傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-467">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="7592c-468">Java 用戶端預設使用 WebSocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-468">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="7592c-469">SignalR Java 用戶端還不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="7592c-469">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="7592c-470">設定無記名身份驗證</span><span class="sxs-lookup"><span data-stu-id="7592c-470">Configure bearer authentication</span></span>

<span data-ttu-id="7592c-471">要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-471">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="7592c-472">在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。</span><span class="sxs-lookup"><span data-stu-id="7592c-472">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="7592c-473">在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-473">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="7592c-474">在這些情況下,訪問權杖作為查詢字串值`access_token`提供。</span><span class="sxs-lookup"><span data-stu-id="7592c-474">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="7592c-475">在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-475">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="7592c-476">在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="7592c-476">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="7592c-477">在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。</span><span class="sxs-lookup"><span data-stu-id="7592c-477">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="7592c-478">[與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html)</span><span class="sxs-lookup"><span data-stu-id="7592c-478">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="7592c-479">調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="7592c-479">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="7592c-480">設定逾時與保持作用選項</span><span class="sxs-lookup"><span data-stu-id="7592c-480">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="7592c-481">`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:</span><span class="sxs-lookup"><span data-stu-id="7592c-481">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="7592c-482">.NET</span><span class="sxs-lookup"><span data-stu-id="7592c-482">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7592c-483">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-483">Option</span></span> | <span data-ttu-id="7592c-484">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-484">Default value</span></span> | <span data-ttu-id="7592c-485">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-485">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="7592c-486">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-486">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-487">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-487">Timeout for server activity.</span></span> <span data-ttu-id="7592c-488">如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-488">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7592c-489">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-489">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-490">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-490">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="7592c-491">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-491">15 seconds</span></span> | <span data-ttu-id="7592c-492">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-492">Timeout for initial server handshake.</span></span> <span data-ttu-id="7592c-493">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-493">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7592c-494">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-494">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-495">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-495">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7592c-496">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-496">15 seconds</span></span> | <span data-ttu-id="7592c-497">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="7592c-497">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7592c-498">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="7592c-498">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7592c-499">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-499">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="7592c-500">在 .NET 用戶端中,超時值`TimeSpan`指定為值。</span><span class="sxs-lookup"><span data-stu-id="7592c-500">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="7592c-501">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7592c-501">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7592c-502">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-502">Option</span></span> | <span data-ttu-id="7592c-503">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-503">Default value</span></span> | <span data-ttu-id="7592c-504">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-504">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="7592c-505">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-505">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-506">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-506">Timeout for server activity.</span></span> <span data-ttu-id="7592c-507">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-507">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="7592c-508">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-508">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-509">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-509">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="7592c-510">15 秒(15,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-510">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="7592c-511">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="7592c-511">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7592c-512">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="7592c-512">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7592c-513">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-513">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="7592c-514">Java</span><span class="sxs-lookup"><span data-stu-id="7592c-514">Java</span></span>](#tab/java)

| <span data-ttu-id="7592c-515">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-515">Option</span></span> | <span data-ttu-id="7592c-516">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-516">Default value</span></span> | <span data-ttu-id="7592c-517">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-517">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="7592c-518">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="7592c-518">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="7592c-519">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-519">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-520">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-520">Timeout for server activity.</span></span> <span data-ttu-id="7592c-521">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-521">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="7592c-522">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-522">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-523">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-523">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="7592c-524">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-524">15 seconds</span></span> | <span data-ttu-id="7592c-525">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-525">Timeout for initial server handshake.</span></span> <span data-ttu-id="7592c-526">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-526">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="7592c-527">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-527">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-528">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-528">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="7592c-529">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="7592c-529">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="7592c-530">15 秒(15,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-530">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="7592c-531">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="7592c-531">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7592c-532">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="7592c-532">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7592c-533">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-533">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="7592c-534">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="7592c-534">Configure additional options</span></span>

<span data-ttu-id="7592c-535">其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:</span><span class="sxs-lookup"><span data-stu-id="7592c-535">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="7592c-536">.NET</span><span class="sxs-lookup"><span data-stu-id="7592c-536">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7592c-537">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-537">.NET Option</span></span> |  <span data-ttu-id="7592c-538">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-538">Default value</span></span> | <span data-ttu-id="7592c-539">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-539">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="7592c-540">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-540">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="7592c-541">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-541">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-542">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-542">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-543">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-543">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="7592c-544">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-544">Empty</span></span> | <span data-ttu-id="7592c-545">要發送到身份驗證請求的 TLS 證書的集合。</span><span class="sxs-lookup"><span data-stu-id="7592c-545">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="7592c-546">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-546">Empty</span></span> | <span data-ttu-id="7592c-547">要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="7592c-547">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="7592c-548">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-548">Empty</span></span> | <span data-ttu-id="7592c-549">要隨每個 HTTP 請求一起發送的認證。</span><span class="sxs-lookup"><span data-stu-id="7592c-549">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="7592c-550">5 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-550">5 seconds</span></span> | <span data-ttu-id="7592c-551">僅限 Web 插座。</span><span class="sxs-lookup"><span data-stu-id="7592c-551">WebSockets only.</span></span> <span data-ttu-id="7592c-552">用戶端關閉後等待伺服器確認關閉請求的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="7592c-552">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="7592c-553">如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-553">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="7592c-554">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-554">Empty</span></span> | <span data-ttu-id="7592c-555">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="7592c-555">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="7592c-556">可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-556">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="7592c-557">不用於 Web 套接字連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-557">Not used for WebSocket connections.</span></span> <span data-ttu-id="7592c-558">此委託必須返回一個非空值,並且它將作為參數接收預設值。</span><span class="sxs-lookup"><span data-stu-id="7592c-558">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="7592c-559">修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。</span><span class="sxs-lookup"><span data-stu-id="7592c-559">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="7592c-560">**替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。**</span><span class="sxs-lookup"><span data-stu-id="7592c-560">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="7592c-561">傳送 HTTP 請求時要使用的 HTTP 代理。</span><span class="sxs-lookup"><span data-stu-id="7592c-561">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="7592c-562">設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="7592c-562">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="7592c-563">這支援使用 Windows 身份驗證。</span><span class="sxs-lookup"><span data-stu-id="7592c-563">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="7592c-564">可用於配置其他 WebSocket 選項的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-564">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="7592c-565">接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。</span><span class="sxs-lookup"><span data-stu-id="7592c-565">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="7592c-566">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7592c-566">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7592c-567">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-567">JavaScript Option</span></span> | <span data-ttu-id="7592c-568">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-568">Default Value</span></span> | <span data-ttu-id="7592c-569">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-569">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="7592c-570">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-570">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="7592c-571">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-571">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-572">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-572">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-573">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-573">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="7592c-574">Java</span><span class="sxs-lookup"><span data-stu-id="7592c-574">Java</span></span>](#tab/java)

| <span data-ttu-id="7592c-575">Java 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-575">Java Option</span></span> | <span data-ttu-id="7592c-576">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-576">Default Value</span></span> | <span data-ttu-id="7592c-577">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-577">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="7592c-578">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-578">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="7592c-579">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-579">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-580">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-580">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-581">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-581">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="7592c-582">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="7592c-582">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="7592c-583">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-583">Empty</span></span> | <span data-ttu-id="7592c-584">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="7592c-584">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="7592c-585">在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:</span><span class="sxs-lookup"><span data-stu-id="7592c-585">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="7592c-586">在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:</span><span class="sxs-lookup"><span data-stu-id="7592c-586">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="7592c-587">在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="7592c-587">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="7592c-588">其他資源</span><span class="sxs-lookup"><span data-stu-id="7592c-588">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="7592c-589">JSON/訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="7592c-589">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="7592c-590">ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="7592c-590">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="7592c-591">每個協定都有序列化配置選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-591">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="7592c-592">JSON 序列化可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置,該方法`Startup.ConfigureServices`可以在方法中的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後添加。</span><span class="sxs-lookup"><span data-stu-id="7592c-592">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7592c-593">該方法`AddJsonProtocol`採用`options`接收 物件的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-593">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="7592c-594">該物件的[Payload 序列化器設定](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性`JsonSerializerSettings`是可用於 配置參數和返回值序列化 JSON.NET 物件。</span><span class="sxs-lookup"><span data-stu-id="7592c-594">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="7592c-595">有關詳細資訊,請參閱[JSON.NET 文檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="7592c-595">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="7592c-596">例如,要將序列化程式配置為使用「PascalCase」屬性名稱,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:</span><span class="sxs-lookup"><span data-stu-id="7592c-596">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="7592c-597">在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="7592c-597">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="7592c-598">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:</span><span class="sxs-lookup"><span data-stu-id="7592c-598">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="7592c-599">此時無法在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="7592c-599">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="7592c-600">訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="7592c-600">MessagePack serialization options</span></span>

<span data-ttu-id="7592c-601">消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-601">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="7592c-602">有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。</span><span class="sxs-lookup"><span data-stu-id="7592c-602">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="7592c-603">此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="7592c-603">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="7592c-604">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="7592c-604">Configure server options</span></span>

<span data-ttu-id="7592c-605">下表描述了用於設定 SignalR 集線器的選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-605">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="7592c-606">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-606">Option</span></span> | <span data-ttu-id="7592c-607">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-607">Default Value</span></span> | <span data-ttu-id="7592c-608">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="7592c-609">30 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-609">30 seconds</span></span> | <span data-ttu-id="7592c-610">如果用戶端未在此間隔內收到消息(包括保持活動狀態),伺服器將考慮用戶端斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-610">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="7592c-611">由於如何實現,實際將客戶端標記為斷開連接的時間可能比此超時間隔長。</span><span class="sxs-lookup"><span data-stu-id="7592c-611">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="7592c-612">建議的值是值的`KeepAliveInterval`兩倍。</span><span class="sxs-lookup"><span data-stu-id="7592c-612">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="7592c-613">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-613">15 seconds</span></span> | <span data-ttu-id="7592c-614">如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。</span><span class="sxs-lookup"><span data-stu-id="7592c-614">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="7592c-615">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-615">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-616">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-616">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7592c-617">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-617">15 seconds</span></span> | <span data-ttu-id="7592c-618">如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。</span><span class="sxs-lookup"><span data-stu-id="7592c-618">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="7592c-619">更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-619">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="7592c-620">建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /</span><span class="sxs-lookup"><span data-stu-id="7592c-620">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="7592c-621">所有已安裝的協定</span><span class="sxs-lookup"><span data-stu-id="7592c-621">All installed protocols</span></span> | <span data-ttu-id="7592c-622">此中心支持的協定。</span><span class="sxs-lookup"><span data-stu-id="7592c-622">Protocols supported by this hub.</span></span> <span data-ttu-id="7592c-623">默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。</span><span class="sxs-lookup"><span data-stu-id="7592c-623">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="7592c-624">如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。</span><span class="sxs-lookup"><span data-stu-id="7592c-624">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="7592c-625">默認值為`false`,因為這些異常消息可能包含敏感資訊。</span><span class="sxs-lookup"><span data-stu-id="7592c-625">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="7592c-626">可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-626">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="7592c-627">單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :</span><span class="sxs-lookup"><span data-stu-id="7592c-627">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="7592c-628">進階 HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="7592c-628">Advanced HTTP configuration options</span></span>

<span data-ttu-id="7592c-629">用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-629">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="7592c-630">這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)在`Startup.Configure`中配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-630">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="7592c-631">下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-631">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="7592c-632">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-632">Option</span></span> | <span data-ttu-id="7592c-633">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-633">Default Value</span></span> | <span data-ttu-id="7592c-634">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-634">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="7592c-635">32 KB</span><span class="sxs-lookup"><span data-stu-id="7592c-635">32 KB</span></span> | <span data-ttu-id="7592c-636">從伺服器緩衝的用戶端接收的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="7592c-636">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="7592c-637">增加此值允許伺服器接收較大的消息,但可能會對記憶體消耗產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="7592c-637">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="7592c-638">數據自動從應用於中心`Authorize`類的屬性中收集。</span><span class="sxs-lookup"><span data-stu-id="7592c-638">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="7592c-639">用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。</span><span class="sxs-lookup"><span data-stu-id="7592c-639">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="7592c-640">32 KB</span><span class="sxs-lookup"><span data-stu-id="7592c-640">32 KB</span></span> | <span data-ttu-id="7592c-641">伺服器緩衝的應用發送的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="7592c-641">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="7592c-642">增加此值允許伺服器發送更大的消息,但可能會對記憶體消耗產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="7592c-642">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="7592c-643">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="7592c-643">All Transports are enabled.</span></span> | <span data-ttu-id="7592c-644">位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-644">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="7592c-645">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="7592c-645">See below.</span></span> | <span data-ttu-id="7592c-646">特定於長輪詢傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-646">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="7592c-647">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="7592c-647">See below.</span></span> | <span data-ttu-id="7592c-648">特定於 WebSocket 傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-648">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="7592c-649">長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-649">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="7592c-650">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-650">Option</span></span> | <span data-ttu-id="7592c-651">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-651">Default Value</span></span> | <span data-ttu-id="7592c-652">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-652">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="7592c-653">90 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-653">90 seconds</span></span> | <span data-ttu-id="7592c-654">伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。</span><span class="sxs-lookup"><span data-stu-id="7592c-654">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="7592c-655">減小此值會導致用戶端更頻繁地發出新的輪詢請求。</span><span class="sxs-lookup"><span data-stu-id="7592c-655">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="7592c-656">WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-656">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="7592c-657">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-657">Option</span></span> | <span data-ttu-id="7592c-658">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-658">Default Value</span></span> | <span data-ttu-id="7592c-659">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-659">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="7592c-660">5 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-660">5 seconds</span></span> | <span data-ttu-id="7592c-661">伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。</span><span class="sxs-lookup"><span data-stu-id="7592c-661">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="7592c-662">可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-662">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="7592c-663">委託接收用戶端請求的值作為輸入,並預期返回所需的值。</span><span class="sxs-lookup"><span data-stu-id="7592c-663">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="7592c-664">設定客戶端選項</span><span class="sxs-lookup"><span data-stu-id="7592c-664">Configure client options</span></span>

<span data-ttu-id="7592c-665">可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。</span><span class="sxs-lookup"><span data-stu-id="7592c-665">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="7592c-666">它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。</span><span class="sxs-lookup"><span data-stu-id="7592c-666">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="7592c-667">設定記錄</span><span class="sxs-lookup"><span data-stu-id="7592c-667">Configure logging</span></span>

<span data-ttu-id="7592c-668">使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="7592c-668">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="7592c-669">日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。</span><span class="sxs-lookup"><span data-stu-id="7592c-669">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="7592c-670">有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。</span><span class="sxs-lookup"><span data-stu-id="7592c-670">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="7592c-671">要註冊日誌記錄提供程式,必須安裝必要的包。</span><span class="sxs-lookup"><span data-stu-id="7592c-671">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="7592c-672">有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。</span><span class="sxs-lookup"><span data-stu-id="7592c-672">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="7592c-673">例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="7592c-673">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="7592c-674">呼叫`AddConsole`式副用:</span><span class="sxs-lookup"><span data-stu-id="7592c-674">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="7592c-675">在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。</span><span class="sxs-lookup"><span data-stu-id="7592c-675">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="7592c-676">提供指示`LogLevel`要生產的日誌消息的最小級別的值。</span><span class="sxs-lookup"><span data-stu-id="7592c-676">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="7592c-677">日誌將寫入瀏覽器控制台視窗。</span><span class="sxs-lookup"><span data-stu-id="7592c-677">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="7592c-678">要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。</span><span class="sxs-lookup"><span data-stu-id="7592c-678">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="7592c-679">關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="7592c-679">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="7592c-680">SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="7592c-680">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="7592c-681">它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。</span><span class="sxs-lookup"><span data-stu-id="7592c-681">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="7592c-682">以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。</span><span class="sxs-lookup"><span data-stu-id="7592c-682">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="7592c-683">如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:</span><span class="sxs-lookup"><span data-stu-id="7592c-683">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="7592c-684">這可以安全地忽略。</span><span class="sxs-lookup"><span data-stu-id="7592c-684">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="7592c-685">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="7592c-685">Configure allowed transports</span></span>

<span data-ttu-id="7592c-686">SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-686">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="7592c-687">值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-687">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="7592c-688">默認情況下,所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="7592c-688">All transports are enabled by default.</span></span>

<span data-ttu-id="7592c-689">例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:</span><span class="sxs-lookup"><span data-stu-id="7592c-689">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="7592c-690">在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:</span><span class="sxs-lookup"><span data-stu-id="7592c-690">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="7592c-691">在此版本中,JAva 用戶端 Websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-691">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="7592c-692">設定無記名身份驗證</span><span class="sxs-lookup"><span data-stu-id="7592c-692">Configure bearer authentication</span></span>

<span data-ttu-id="7592c-693">要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-693">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="7592c-694">在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。</span><span class="sxs-lookup"><span data-stu-id="7592c-694">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="7592c-695">在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-695">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="7592c-696">在這些情況下,訪問權杖作為查詢字串值`access_token`提供。</span><span class="sxs-lookup"><span data-stu-id="7592c-696">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="7592c-697">在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-697">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="7592c-698">在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="7592c-698">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="7592c-699">在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。</span><span class="sxs-lookup"><span data-stu-id="7592c-699">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="7592c-700">[與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html)</span><span class="sxs-lookup"><span data-stu-id="7592c-700">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="7592c-701">調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="7592c-701">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="7592c-702">設定逾時與保持作用選項</span><span class="sxs-lookup"><span data-stu-id="7592c-702">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="7592c-703">`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:</span><span class="sxs-lookup"><span data-stu-id="7592c-703">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="7592c-704">.NET</span><span class="sxs-lookup"><span data-stu-id="7592c-704">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7592c-705">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-705">Option</span></span> | <span data-ttu-id="7592c-706">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-706">Default value</span></span> | <span data-ttu-id="7592c-707">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-707">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="7592c-708">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-708">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-709">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-709">Timeout for server activity.</span></span> <span data-ttu-id="7592c-710">如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-710">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7592c-711">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-711">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-712">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-712">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="7592c-713">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-713">15 seconds</span></span> | <span data-ttu-id="7592c-714">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-714">Timeout for initial server handshake.</span></span> <span data-ttu-id="7592c-715">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-715">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7592c-716">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-716">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-717">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-717">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7592c-718">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-718">15 seconds</span></span> | <span data-ttu-id="7592c-719">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="7592c-719">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7592c-720">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="7592c-720">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7592c-721">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-721">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="7592c-722">在 .NET 用戶端中,超時值`TimeSpan`指定為值。</span><span class="sxs-lookup"><span data-stu-id="7592c-722">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="7592c-723">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7592c-723">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7592c-724">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-724">Option</span></span> | <span data-ttu-id="7592c-725">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-725">Default value</span></span> | <span data-ttu-id="7592c-726">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-726">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="7592c-727">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-727">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-728">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-728">Timeout for server activity.</span></span> <span data-ttu-id="7592c-729">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-729">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="7592c-730">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-730">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-731">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-731">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="7592c-732">15 秒(15,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-732">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="7592c-733">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="7592c-733">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7592c-734">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="7592c-734">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7592c-735">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-735">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="7592c-736">Java</span><span class="sxs-lookup"><span data-stu-id="7592c-736">Java</span></span>](#tab/java)

| <span data-ttu-id="7592c-737">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-737">Option</span></span> | <span data-ttu-id="7592c-738">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-738">Default value</span></span> | <span data-ttu-id="7592c-739">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-739">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="7592c-740">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="7592c-740">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="7592c-741">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-741">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-742">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-742">Timeout for server activity.</span></span> <span data-ttu-id="7592c-743">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-743">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="7592c-744">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-744">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-745">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-745">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="7592c-746">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-746">15 seconds</span></span> | <span data-ttu-id="7592c-747">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-747">Timeout for initial server handshake.</span></span> <span data-ttu-id="7592c-748">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-748">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="7592c-749">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-749">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-750">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-750">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="7592c-751">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="7592c-751">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="7592c-752">15 秒(15,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-752">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="7592c-753">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="7592c-753">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7592c-754">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="7592c-754">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7592c-755">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-755">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="7592c-756">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="7592c-756">Configure additional options</span></span>

<span data-ttu-id="7592c-757">其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:</span><span class="sxs-lookup"><span data-stu-id="7592c-757">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="7592c-758">.NET</span><span class="sxs-lookup"><span data-stu-id="7592c-758">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7592c-759">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-759">.NET Option</span></span> |  <span data-ttu-id="7592c-760">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-760">Default value</span></span> | <span data-ttu-id="7592c-761">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-761">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="7592c-762">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-762">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="7592c-763">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-763">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-764">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-764">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-765">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-765">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="7592c-766">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-766">Empty</span></span> | <span data-ttu-id="7592c-767">要發送到身份驗證請求的 TLS 證書的集合。</span><span class="sxs-lookup"><span data-stu-id="7592c-767">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="7592c-768">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-768">Empty</span></span> | <span data-ttu-id="7592c-769">要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="7592c-769">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="7592c-770">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-770">Empty</span></span> | <span data-ttu-id="7592c-771">要隨每個 HTTP 請求一起發送的認證。</span><span class="sxs-lookup"><span data-stu-id="7592c-771">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="7592c-772">5 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-772">5 seconds</span></span> | <span data-ttu-id="7592c-773">僅限 Web 插座。</span><span class="sxs-lookup"><span data-stu-id="7592c-773">WebSockets only.</span></span> <span data-ttu-id="7592c-774">用戶端關閉後等待伺服器確認關閉請求的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="7592c-774">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="7592c-775">如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-775">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="7592c-776">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-776">Empty</span></span> | <span data-ttu-id="7592c-777">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="7592c-777">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="7592c-778">可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-778">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="7592c-779">不用於 Web 套接字連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-779">Not used for WebSocket connections.</span></span> <span data-ttu-id="7592c-780">此委託必須返回一個非空值,並且它將作為參數接收預設值。</span><span class="sxs-lookup"><span data-stu-id="7592c-780">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="7592c-781">修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。</span><span class="sxs-lookup"><span data-stu-id="7592c-781">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="7592c-782">**替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。**</span><span class="sxs-lookup"><span data-stu-id="7592c-782">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="7592c-783">傳送 HTTP 請求時要使用的 HTTP 代理。</span><span class="sxs-lookup"><span data-stu-id="7592c-783">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="7592c-784">設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="7592c-784">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="7592c-785">這支援使用 Windows 身份驗證。</span><span class="sxs-lookup"><span data-stu-id="7592c-785">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="7592c-786">可用於配置其他 WebSocket 選項的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-786">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="7592c-787">接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。</span><span class="sxs-lookup"><span data-stu-id="7592c-787">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="7592c-788">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7592c-788">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7592c-789">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-789">JavaScript Option</span></span> | <span data-ttu-id="7592c-790">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-790">Default Value</span></span> | <span data-ttu-id="7592c-791">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-791">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="7592c-792">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-792">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="7592c-793">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-793">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-794">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-794">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-795">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-795">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="7592c-796">Java</span><span class="sxs-lookup"><span data-stu-id="7592c-796">Java</span></span>](#tab/java)

| <span data-ttu-id="7592c-797">Java 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-797">Java Option</span></span> | <span data-ttu-id="7592c-798">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-798">Default Value</span></span> | <span data-ttu-id="7592c-799">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-799">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="7592c-800">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-800">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="7592c-801">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-801">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-802">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-802">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-803">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-803">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="7592c-804">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="7592c-804">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="7592c-805">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-805">Empty</span></span> | <span data-ttu-id="7592c-806">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="7592c-806">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="7592c-807">在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:</span><span class="sxs-lookup"><span data-stu-id="7592c-807">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="7592c-808">在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:</span><span class="sxs-lookup"><span data-stu-id="7592c-808">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="7592c-809">在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="7592c-809">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="7592c-810">其他資源</span><span class="sxs-lookup"><span data-stu-id="7592c-810">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="7592c-811">JSON/訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="7592c-811">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="7592c-812">ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="7592c-812">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="7592c-813">每個協定都有序列化配置選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-813">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="7592c-814">JSON 序列化可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置,該方法`Startup.ConfigureServices`可以在方法中的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後添加。</span><span class="sxs-lookup"><span data-stu-id="7592c-814">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7592c-815">該方法`AddJsonProtocol`採用`options`接收 物件的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-815">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="7592c-816">該物件的[Payload 序列化器設定](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性`JsonSerializerSettings`是可用於 配置參數和返回值序列化 JSON.NET 物件。</span><span class="sxs-lookup"><span data-stu-id="7592c-816">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="7592c-817">有關詳細資訊,請參閱[JSON.NET 文檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="7592c-817">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="7592c-818">例如,要將序列化程式配置為使用「PascalCase」屬性名稱,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:</span><span class="sxs-lookup"><span data-stu-id="7592c-818">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="7592c-819">在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="7592c-819">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="7592c-820">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:</span><span class="sxs-lookup"><span data-stu-id="7592c-820">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="7592c-821">此時無法在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="7592c-821">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="7592c-822">訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="7592c-822">MessagePack serialization options</span></span>

<span data-ttu-id="7592c-823">消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-823">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="7592c-824">有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。</span><span class="sxs-lookup"><span data-stu-id="7592c-824">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="7592c-825">此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="7592c-825">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="7592c-826">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="7592c-826">Configure server options</span></span>

<span data-ttu-id="7592c-827">下表描述了用於設定 SignalR 集線器的選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-827">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="7592c-828">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-828">Option</span></span> | <span data-ttu-id="7592c-829">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-829">Default Value</span></span> | <span data-ttu-id="7592c-830">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-830">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="7592c-831">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-831">15 seconds</span></span> | <span data-ttu-id="7592c-832">如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。</span><span class="sxs-lookup"><span data-stu-id="7592c-832">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="7592c-833">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-833">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-834">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-834">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7592c-835">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-835">15 seconds</span></span> | <span data-ttu-id="7592c-836">如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。</span><span class="sxs-lookup"><span data-stu-id="7592c-836">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="7592c-837">更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-837">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="7592c-838">建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /</span><span class="sxs-lookup"><span data-stu-id="7592c-838">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="7592c-839">所有已安裝的協定</span><span class="sxs-lookup"><span data-stu-id="7592c-839">All installed protocols</span></span> | <span data-ttu-id="7592c-840">此中心支持的協定。</span><span class="sxs-lookup"><span data-stu-id="7592c-840">Protocols supported by this hub.</span></span> <span data-ttu-id="7592c-841">默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。</span><span class="sxs-lookup"><span data-stu-id="7592c-841">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="7592c-842">如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。</span><span class="sxs-lookup"><span data-stu-id="7592c-842">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="7592c-843">默認值為`false`,因為這些異常消息可能包含敏感資訊。</span><span class="sxs-lookup"><span data-stu-id="7592c-843">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="7592c-844">可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-844">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="7592c-845">單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :</span><span class="sxs-lookup"><span data-stu-id="7592c-845">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="7592c-846">進階 HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="7592c-846">Advanced HTTP configuration options</span></span>

<span data-ttu-id="7592c-847">用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-847">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="7592c-848">這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)在`Startup.Configure`中配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-848">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="7592c-849">下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-849">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="7592c-850">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-850">Option</span></span> | <span data-ttu-id="7592c-851">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-851">Default Value</span></span> | <span data-ttu-id="7592c-852">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-852">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="7592c-853">32 KB</span><span class="sxs-lookup"><span data-stu-id="7592c-853">32 KB</span></span> | <span data-ttu-id="7592c-854">從伺服器緩衝的用戶端接收的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="7592c-854">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="7592c-855">增加此值允許伺服器接收較大的消息,但可能會對記憶體消耗產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="7592c-855">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="7592c-856">數據自動從應用於中心`Authorize`類的屬性中收集。</span><span class="sxs-lookup"><span data-stu-id="7592c-856">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="7592c-857">用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。</span><span class="sxs-lookup"><span data-stu-id="7592c-857">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="7592c-858">32 KB</span><span class="sxs-lookup"><span data-stu-id="7592c-858">32 KB</span></span> | <span data-ttu-id="7592c-859">伺服器緩衝的應用發送的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="7592c-859">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="7592c-860">增加此值允許伺服器發送更大的消息,但可能會對記憶體消耗產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="7592c-860">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="7592c-861">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="7592c-861">All Transports are enabled.</span></span> | <span data-ttu-id="7592c-862">位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-862">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="7592c-863">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="7592c-863">See below.</span></span> | <span data-ttu-id="7592c-864">特定於長輪詢傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-864">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="7592c-865">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="7592c-865">See below.</span></span> | <span data-ttu-id="7592c-866">特定於 WebSocket 傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="7592c-866">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="7592c-867">長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-867">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="7592c-868">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-868">Option</span></span> | <span data-ttu-id="7592c-869">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-869">Default Value</span></span> | <span data-ttu-id="7592c-870">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-870">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="7592c-871">90 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-871">90 seconds</span></span> | <span data-ttu-id="7592c-872">伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。</span><span class="sxs-lookup"><span data-stu-id="7592c-872">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="7592c-873">減小此值會導致用戶端更頻繁地發出新的輪詢請求。</span><span class="sxs-lookup"><span data-stu-id="7592c-873">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="7592c-874">WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-874">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="7592c-875">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-875">Option</span></span> | <span data-ttu-id="7592c-876">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-876">Default Value</span></span> | <span data-ttu-id="7592c-877">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-877">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="7592c-878">5 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-878">5 seconds</span></span> | <span data-ttu-id="7592c-879">伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。</span><span class="sxs-lookup"><span data-stu-id="7592c-879">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="7592c-880">可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-880">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="7592c-881">委託接收用戶端請求的值作為輸入,並預期返回所需的值。</span><span class="sxs-lookup"><span data-stu-id="7592c-881">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="7592c-882">設定客戶端選項</span><span class="sxs-lookup"><span data-stu-id="7592c-882">Configure client options</span></span>

<span data-ttu-id="7592c-883">可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。</span><span class="sxs-lookup"><span data-stu-id="7592c-883">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="7592c-884">它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。</span><span class="sxs-lookup"><span data-stu-id="7592c-884">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="7592c-885">設定記錄</span><span class="sxs-lookup"><span data-stu-id="7592c-885">Configure logging</span></span>

<span data-ttu-id="7592c-886">使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="7592c-886">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="7592c-887">日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。</span><span class="sxs-lookup"><span data-stu-id="7592c-887">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="7592c-888">有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。</span><span class="sxs-lookup"><span data-stu-id="7592c-888">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="7592c-889">要註冊日誌記錄提供程式,必須安裝必要的包。</span><span class="sxs-lookup"><span data-stu-id="7592c-889">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="7592c-890">有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。</span><span class="sxs-lookup"><span data-stu-id="7592c-890">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="7592c-891">例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="7592c-891">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="7592c-892">呼叫`AddConsole`式副用:</span><span class="sxs-lookup"><span data-stu-id="7592c-892">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="7592c-893">在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。</span><span class="sxs-lookup"><span data-stu-id="7592c-893">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="7592c-894">提供指示`LogLevel`要生產的日誌消息的最小級別的值。</span><span class="sxs-lookup"><span data-stu-id="7592c-894">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="7592c-895">日誌將寫入瀏覽器控制台視窗。</span><span class="sxs-lookup"><span data-stu-id="7592c-895">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="7592c-896">要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。</span><span class="sxs-lookup"><span data-stu-id="7592c-896">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="7592c-897">關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="7592c-897">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="7592c-898">SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="7592c-898">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="7592c-899">它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。</span><span class="sxs-lookup"><span data-stu-id="7592c-899">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="7592c-900">以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。</span><span class="sxs-lookup"><span data-stu-id="7592c-900">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="7592c-901">如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:</span><span class="sxs-lookup"><span data-stu-id="7592c-901">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="7592c-902">這可以安全地忽略。</span><span class="sxs-lookup"><span data-stu-id="7592c-902">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="7592c-903">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="7592c-903">Configure allowed transports</span></span>

<span data-ttu-id="7592c-904">SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。</span><span class="sxs-lookup"><span data-stu-id="7592c-904">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="7592c-905">值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="7592c-905">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="7592c-906">默認情況下,所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="7592c-906">All transports are enabled by default.</span></span>

<span data-ttu-id="7592c-907">例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:</span><span class="sxs-lookup"><span data-stu-id="7592c-907">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="7592c-908">在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:</span><span class="sxs-lookup"><span data-stu-id="7592c-908">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="7592c-909">設定無記名身份驗證</span><span class="sxs-lookup"><span data-stu-id="7592c-909">Configure bearer authentication</span></span>

<span data-ttu-id="7592c-910">要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-910">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="7592c-911">在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。</span><span class="sxs-lookup"><span data-stu-id="7592c-911">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="7592c-912">在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-912">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="7592c-913">在這些情況下,訪問權杖作為查詢字串值`access_token`提供。</span><span class="sxs-lookup"><span data-stu-id="7592c-913">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="7592c-914">在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:</span><span class="sxs-lookup"><span data-stu-id="7592c-914">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="7592c-915">在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="7592c-915">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="7592c-916">在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。</span><span class="sxs-lookup"><span data-stu-id="7592c-916">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="7592c-917">[與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html)</span><span class="sxs-lookup"><span data-stu-id="7592c-917">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="7592c-918">調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="7592c-918">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="7592c-919">設定逾時與保持作用選項</span><span class="sxs-lookup"><span data-stu-id="7592c-919">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="7592c-920">`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:</span><span class="sxs-lookup"><span data-stu-id="7592c-920">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="7592c-921">.NET</span><span class="sxs-lookup"><span data-stu-id="7592c-921">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7592c-922">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-922">Option</span></span> | <span data-ttu-id="7592c-923">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-923">Default value</span></span> | <span data-ttu-id="7592c-924">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-924">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="7592c-925">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-925">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-926">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-926">Timeout for server activity.</span></span> <span data-ttu-id="7592c-927">如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-927">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7592c-928">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-928">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-929">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-929">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="7592c-930">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-930">15 seconds</span></span> | <span data-ttu-id="7592c-931">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-931">Timeout for initial server handshake.</span></span> <span data-ttu-id="7592c-932">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="7592c-932">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7592c-933">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-933">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-934">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-934">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="7592c-935">在 .NET 用戶端中,超時值`TimeSpan`指定為值。</span><span class="sxs-lookup"><span data-stu-id="7592c-935">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="7592c-936">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7592c-936">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7592c-937">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-937">Option</span></span> | <span data-ttu-id="7592c-938">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-938">Default value</span></span> | <span data-ttu-id="7592c-939">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-939">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="7592c-940">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-940">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-941">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-941">Timeout for server activity.</span></span> <span data-ttu-id="7592c-942">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-942">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="7592c-943">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-943">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-944">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-944">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="java"></a>[<span data-ttu-id="7592c-945">Java</span><span class="sxs-lookup"><span data-stu-id="7592c-945">Java</span></span>](#tab/java)

| <span data-ttu-id="7592c-946">選項</span><span class="sxs-lookup"><span data-stu-id="7592c-946">Option</span></span> | <span data-ttu-id="7592c-947">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-947">Default value</span></span> | <span data-ttu-id="7592c-948">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-948">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="7592c-949">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="7592c-949">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="7592c-950">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="7592c-950">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7592c-951">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-951">Timeout for server activity.</span></span> <span data-ttu-id="7592c-952">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-952">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="7592c-953">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="7592c-953">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7592c-954">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="7592c-954">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="7592c-955">15 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-955">15 seconds</span></span> | <span data-ttu-id="7592c-956">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="7592c-956">Timeout for initial server handshake.</span></span> <span data-ttu-id="7592c-957">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="7592c-957">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="7592c-958">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="7592c-958">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7592c-959">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="7592c-959">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="7592c-960">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="7592c-960">Configure additional options</span></span>

<span data-ttu-id="7592c-961">其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:</span><span class="sxs-lookup"><span data-stu-id="7592c-961">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="7592c-962">.NET</span><span class="sxs-lookup"><span data-stu-id="7592c-962">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7592c-963">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-963">.NET Option</span></span> |  <span data-ttu-id="7592c-964">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-964">Default value</span></span> | <span data-ttu-id="7592c-965">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-965">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="7592c-966">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-966">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="7592c-967">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-967">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-968">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-968">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-969">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-969">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="7592c-970">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-970">Empty</span></span> | <span data-ttu-id="7592c-971">要發送到身份驗證請求的 TLS 證書的集合。</span><span class="sxs-lookup"><span data-stu-id="7592c-971">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="7592c-972">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-972">Empty</span></span> | <span data-ttu-id="7592c-973">要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="7592c-973">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="7592c-974">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-974">Empty</span></span> | <span data-ttu-id="7592c-975">要隨每個 HTTP 請求一起發送的認證。</span><span class="sxs-lookup"><span data-stu-id="7592c-975">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="7592c-976">5 秒</span><span class="sxs-lookup"><span data-stu-id="7592c-976">5 seconds</span></span> | <span data-ttu-id="7592c-977">僅限 Web 插座。</span><span class="sxs-lookup"><span data-stu-id="7592c-977">WebSockets only.</span></span> <span data-ttu-id="7592c-978">用戶端關閉後等待伺服器確認關閉請求的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="7592c-978">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="7592c-979">如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-979">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="7592c-980">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-980">Empty</span></span> | <span data-ttu-id="7592c-981">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="7592c-981">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="7592c-982">可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-982">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="7592c-983">不用於 Web 套接字連接。</span><span class="sxs-lookup"><span data-stu-id="7592c-983">Not used for WebSocket connections.</span></span> <span data-ttu-id="7592c-984">此委託必須返回一個非空值,並且它將作為參數接收預設值。</span><span class="sxs-lookup"><span data-stu-id="7592c-984">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="7592c-985">修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。</span><span class="sxs-lookup"><span data-stu-id="7592c-985">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="7592c-986">**替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。**</span><span class="sxs-lookup"><span data-stu-id="7592c-986">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="7592c-987">傳送 HTTP 請求時要使用的 HTTP 代理。</span><span class="sxs-lookup"><span data-stu-id="7592c-987">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="7592c-988">設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="7592c-988">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="7592c-989">這支援使用 Windows 身份驗證。</span><span class="sxs-lookup"><span data-stu-id="7592c-989">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="7592c-990">可用於配置其他 WebSocket 選項的委託。</span><span class="sxs-lookup"><span data-stu-id="7592c-990">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="7592c-991">接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。</span><span class="sxs-lookup"><span data-stu-id="7592c-991">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="7592c-992">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7592c-992">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7592c-993">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-993">JavaScript Option</span></span> | <span data-ttu-id="7592c-994">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-994">Default Value</span></span> | <span data-ttu-id="7592c-995">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-995">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="7592c-996">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-996">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="7592c-997">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-997">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-998">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-998">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-999">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-999">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="7592c-1000">Java</span><span class="sxs-lookup"><span data-stu-id="7592c-1000">Java</span></span>](#tab/java)

| <span data-ttu-id="7592c-1001">Java 選項</span><span class="sxs-lookup"><span data-stu-id="7592c-1001">Java Option</span></span> | <span data-ttu-id="7592c-1002">預設值</span><span class="sxs-lookup"><span data-stu-id="7592c-1002">Default Value</span></span> | <span data-ttu-id="7592c-1003">描述</span><span class="sxs-lookup"><span data-stu-id="7592c-1003">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="7592c-1004">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="7592c-1004">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="7592c-1005">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="7592c-1005">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7592c-1006">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="7592c-1006">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7592c-1007">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="7592c-1007">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="7592c-1008">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="7592c-1008">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="7592c-1009">空白</span><span class="sxs-lookup"><span data-stu-id="7592c-1009">Empty</span></span> | <span data-ttu-id="7592c-1010">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="7592c-1010">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="7592c-1011">在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:</span><span class="sxs-lookup"><span data-stu-id="7592c-1011">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="7592c-1012">在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:</span><span class="sxs-lookup"><span data-stu-id="7592c-1012">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="7592c-1013">在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="7592c-1013">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="7592c-1014">其他資源</span><span class="sxs-lookup"><span data-stu-id="7592c-1014">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
