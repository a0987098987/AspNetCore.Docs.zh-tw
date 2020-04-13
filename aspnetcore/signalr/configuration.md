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
ms.openlocfilehash: 2e9fda6d57986171fc375a2e0fdebf9e111218e0
ms.sourcegitcommit: 6f1b516e0c899a49afe9a29044a2383ce2ada3c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2020
ms.locfileid: "81223981"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="8c313-103">ASP.NET Core SignalR 組態</span><span class="sxs-lookup"><span data-stu-id="8c313-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="8c313-104">JSON/訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="8c313-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="8c313-105">ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="8c313-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="8c313-106">每個協定都有序列化配置選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="8c313-107">JSON 序列化可以使用[AddJson 協定](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置。</span><span class="sxs-lookup"><span data-stu-id="8c313-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="8c313-108">`AddJsonProtocol`可以在`Startup.ConfigureServices`[中新增SignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)後新增 。</span><span class="sxs-lookup"><span data-stu-id="8c313-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8c313-109">該方法`AddJsonProtocol`採用`options`接收 物件的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="8c313-110">該物件的[Payload 序列化器選項](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是`System.Text.Json`<xref:System.Text.Json.JsonSerializerOptions>可用於配置參數和返回值序列化的物件。</span><span class="sxs-lookup"><span data-stu-id="8c313-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="8c313-111">關於詳細資訊,請參閱[系統.Text.Json 文件](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="8c313-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="8c313-112">例如,要將序列化程式設定為不變更屬性名稱的大小寫,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:</span><span class="sxs-lookup"><span data-stu-id="8c313-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    });
```

<span data-ttu-id="8c313-113">在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="8c313-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="8c313-114">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:</span><span class="sxs-lookup"><span data-stu-id="8c313-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="8c313-115">此時無法在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="8c313-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="8c313-116">切換到牛頓軟. Json</span><span class="sxs-lookup"><span data-stu-id="8c313-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="8c313-117">如果您需要在 中不`Newtonsoft.Json`支援`System.Text.Json`的功能 ,請參閱[切換到 Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson)。</span><span class="sxs-lookup"><span data-stu-id="8c313-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="8c313-118">訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="8c313-118">MessagePack serialization options</span></span>

<span data-ttu-id="8c313-119">消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。</span><span class="sxs-lookup"><span data-stu-id="8c313-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="8c313-120">有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。</span><span class="sxs-lookup"><span data-stu-id="8c313-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="8c313-121">此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="8c313-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="8c313-122">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="8c313-122">Configure server options</span></span>

<span data-ttu-id="8c313-123">下表描述了用於設定 SignalR 集線器的選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="8c313-124">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-124">Option</span></span> | <span data-ttu-id="8c313-125">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-125">Default Value</span></span> | <span data-ttu-id="8c313-126">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="8c313-127">30 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-127">30 seconds</span></span> | <span data-ttu-id="8c313-128">如果用戶端未在此間隔內收到消息(包括保持活動狀態),伺服器將考慮用戶端斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="8c313-129">由於如何實現,實際將客戶端標記為斷開連接的時間可能比此超時間隔長。</span><span class="sxs-lookup"><span data-stu-id="8c313-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="8c313-130">建議的值是值的`KeepAliveInterval`兩倍。</span><span class="sxs-lookup"><span data-stu-id="8c313-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="8c313-131">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-131">15 seconds</span></span> | <span data-ttu-id="8c313-132">如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。</span><span class="sxs-lookup"><span data-stu-id="8c313-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="8c313-133">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8c313-134">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="8c313-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="8c313-135">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-135">15 seconds</span></span> | <span data-ttu-id="8c313-136">如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。</span><span class="sxs-lookup"><span data-stu-id="8c313-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="8c313-137">更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="8c313-138">建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /</span><span class="sxs-lookup"><span data-stu-id="8c313-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="8c313-139">所有已安裝的協定</span><span class="sxs-lookup"><span data-stu-id="8c313-139">All installed protocols</span></span> | <span data-ttu-id="8c313-140">此中心支持的協定。</span><span class="sxs-lookup"><span data-stu-id="8c313-140">Protocols supported by this hub.</span></span> <span data-ttu-id="8c313-141">默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。</span><span class="sxs-lookup"><span data-stu-id="8c313-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="8c313-142">如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。</span><span class="sxs-lookup"><span data-stu-id="8c313-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="8c313-143">默認值為`false`,因為這些異常消息可能包含敏感資訊。</span><span class="sxs-lookup"><span data-stu-id="8c313-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="8c313-144">可緩衝用戶端上載流的最大項數。</span><span class="sxs-lookup"><span data-stu-id="8c313-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="8c313-145">如果達到此限制,則在伺服器處理流項之前將阻止調用的處理。</span><span class="sxs-lookup"><span data-stu-id="8c313-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="8c313-146">32 KB</span><span class="sxs-lookup"><span data-stu-id="8c313-146">32 KB</span></span> | <span data-ttu-id="8c313-147">單個傳入中心消息的最大大小。</span><span class="sxs-lookup"><span data-stu-id="8c313-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="8c313-148">可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="8c313-149">單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :</span><span class="sxs-lookup"><span data-stu-id="8c313-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="8c313-150">進階 HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="8c313-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="8c313-151">用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="8c313-152">這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub)在`Startup.Configure`中配置。</span><span class="sxs-lookup"><span data-stu-id="8c313-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="8c313-153">下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="8c313-154">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-154">Option</span></span> | <span data-ttu-id="8c313-155">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-155">Default Value</span></span> | <span data-ttu-id="8c313-156">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="8c313-157">32 KB</span><span class="sxs-lookup"><span data-stu-id="8c313-157">32 KB</span></span> | <span data-ttu-id="8c313-158">在應用背壓之前,伺服器緩衝的用戶端接收的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="8c313-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="8c313-159">增加此值允許伺服器在不施加背壓的情況下更快地接收更大的消息,但會增加記憶體消耗。</span><span class="sxs-lookup"><span data-stu-id="8c313-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="8c313-160">數據自動從應用於中心`Authorize`類的屬性中收集。</span><span class="sxs-lookup"><span data-stu-id="8c313-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="8c313-161">用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。</span><span class="sxs-lookup"><span data-stu-id="8c313-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="8c313-162">32 KB</span><span class="sxs-lookup"><span data-stu-id="8c313-162">32 KB</span></span> | <span data-ttu-id="8c313-163">在觀察背壓之前,伺服器緩衝的應用發送的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="8c313-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="8c313-164">增加此值允許伺服器更快地緩衝較大的郵件,而無需等待回壓,但會增加記憶體消耗。</span><span class="sxs-lookup"><span data-stu-id="8c313-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="8c313-165">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="8c313-165">All Transports are enabled.</span></span> | <span data-ttu-id="8c313-166">位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="8c313-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="8c313-167">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="8c313-167">See below.</span></span> | <span data-ttu-id="8c313-168">特定於長輪詢傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="8c313-169">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="8c313-169">See below.</span></span> | <span data-ttu-id="8c313-170">特定於 WebSocket 傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-170">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="8c313-171">長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-171">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="8c313-172">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-172">Option</span></span> | <span data-ttu-id="8c313-173">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-173">Default Value</span></span> | <span data-ttu-id="8c313-174">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="8c313-175">90 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-175">90 seconds</span></span> | <span data-ttu-id="8c313-176">伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。</span><span class="sxs-lookup"><span data-stu-id="8c313-176">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="8c313-177">減小此值會導致用戶端更頻繁地發出新的輪詢請求。</span><span class="sxs-lookup"><span data-stu-id="8c313-177">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="8c313-178">WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-178">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="8c313-179">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-179">Option</span></span> | <span data-ttu-id="8c313-180">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-180">Default Value</span></span> | <span data-ttu-id="8c313-181">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-181">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="8c313-182">5 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-182">5 seconds</span></span> | <span data-ttu-id="8c313-183">伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。</span><span class="sxs-lookup"><span data-stu-id="8c313-183">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="8c313-184">可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-184">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="8c313-185">委託接收用戶端請求的值作為輸入,並預期返回所需的值。</span><span class="sxs-lookup"><span data-stu-id="8c313-185">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="8c313-186">設定客戶端選項</span><span class="sxs-lookup"><span data-stu-id="8c313-186">Configure client options</span></span>

<span data-ttu-id="8c313-187">可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。</span><span class="sxs-lookup"><span data-stu-id="8c313-187">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="8c313-188">它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。</span><span class="sxs-lookup"><span data-stu-id="8c313-188">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="8c313-189">設定記錄</span><span class="sxs-lookup"><span data-stu-id="8c313-189">Configure logging</span></span>

<span data-ttu-id="8c313-190">使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="8c313-190">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="8c313-191">日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。</span><span class="sxs-lookup"><span data-stu-id="8c313-191">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="8c313-192">有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。</span><span class="sxs-lookup"><span data-stu-id="8c313-192">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="8c313-193">要註冊日誌記錄提供程式,必須安裝必要的包。</span><span class="sxs-lookup"><span data-stu-id="8c313-193">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="8c313-194">有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。</span><span class="sxs-lookup"><span data-stu-id="8c313-194">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="8c313-195">例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="8c313-195">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="8c313-196">呼叫`AddConsole`式副用:</span><span class="sxs-lookup"><span data-stu-id="8c313-196">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="8c313-197">在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。</span><span class="sxs-lookup"><span data-stu-id="8c313-197">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="8c313-198">提供指示`LogLevel`要生產的日誌消息的最小級別的值。</span><span class="sxs-lookup"><span data-stu-id="8c313-198">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="8c313-199">日誌將寫入瀏覽器控制台視窗。</span><span class="sxs-lookup"><span data-stu-id="8c313-199">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="8c313-200">還可以提供表示`LogLevel`日誌級別`string`名稱的值,而不是值。</span><span class="sxs-lookup"><span data-stu-id="8c313-200">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="8c313-201">這在無法存`LogLevel`取 常量的環境中配置 SignalR 日誌記錄時非常有用。</span><span class="sxs-lookup"><span data-stu-id="8c313-201">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="8c313-202">下表列出了可用的日誌級別。</span><span class="sxs-lookup"><span data-stu-id="8c313-202">The following table lists the available log levels.</span></span> <span data-ttu-id="8c313-203">您提供的值來`configureLogging`設置將記錄的**最小**紀錄級別。</span><span class="sxs-lookup"><span data-stu-id="8c313-203">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="8c313-204">將紀錄在此等級紀錄的訊息,**或表中列出的訊息等級**。</span><span class="sxs-lookup"><span data-stu-id="8c313-204">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="8c313-205">String</span><span class="sxs-lookup"><span data-stu-id="8c313-205">String</span></span>                      | <span data-ttu-id="8c313-206">LogLevel</span><span class="sxs-lookup"><span data-stu-id="8c313-206">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="8c313-207">`info`**或**`information`</span><span class="sxs-lookup"><span data-stu-id="8c313-207">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="8c313-208">`warn`**或**`warning`</span><span class="sxs-lookup"><span data-stu-id="8c313-208">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="8c313-209">要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。</span><span class="sxs-lookup"><span data-stu-id="8c313-209">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="8c313-210">關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="8c313-210">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="8c313-211">SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="8c313-211">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="8c313-212">它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。</span><span class="sxs-lookup"><span data-stu-id="8c313-212">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="8c313-213">以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。</span><span class="sxs-lookup"><span data-stu-id="8c313-213">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="8c313-214">如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:</span><span class="sxs-lookup"><span data-stu-id="8c313-214">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="8c313-215">這可以安全地忽略。</span><span class="sxs-lookup"><span data-stu-id="8c313-215">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="8c313-216">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="8c313-216">Configure allowed transports</span></span>

<span data-ttu-id="8c313-217">SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。</span><span class="sxs-lookup"><span data-stu-id="8c313-217">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="8c313-218">值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="8c313-218">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="8c313-219">默認情況下,所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="8c313-219">All transports are enabled by default.</span></span>

<span data-ttu-id="8c313-220">例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:</span><span class="sxs-lookup"><span data-stu-id="8c313-220">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="8c313-221">在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:</span><span class="sxs-lookup"><span data-stu-id="8c313-221">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="8c313-222">在此版本中,JAva 用戶端 Websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="8c313-222">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="8c313-223">在 Java 用戶端中,`withTransport`使用上的方法選擇`HttpHubConnectionBuilder`傳輸。</span><span class="sxs-lookup"><span data-stu-id="8c313-223">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="8c313-224">Java 用戶端預設使用 WebSocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="8c313-224">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="8c313-225">SignalR Java 用戶端還不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="8c313-225">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="8c313-226">設定無記名身份驗證</span><span class="sxs-lookup"><span data-stu-id="8c313-226">Configure bearer authentication</span></span>

<span data-ttu-id="8c313-227">要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-227">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="8c313-228">在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。</span><span class="sxs-lookup"><span data-stu-id="8c313-228">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="8c313-229">在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。</span><span class="sxs-lookup"><span data-stu-id="8c313-229">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="8c313-230">在這些情況下,訪問權杖作為查詢字串值`access_token`提供。</span><span class="sxs-lookup"><span data-stu-id="8c313-230">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="8c313-231">在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-231">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="8c313-232">在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="8c313-232">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="8c313-233">在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。</span><span class="sxs-lookup"><span data-stu-id="8c313-233">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="8c313-234">[與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html)</span><span class="sxs-lookup"><span data-stu-id="8c313-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="8c313-235">調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="8c313-235">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="8c313-236">設定逾時與保持作用選項</span><span class="sxs-lookup"><span data-stu-id="8c313-236">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="8c313-237">`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:</span><span class="sxs-lookup"><span data-stu-id="8c313-237">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="8c313-238">.NET</span><span class="sxs-lookup"><span data-stu-id="8c313-238">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="8c313-239">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-239">Option</span></span> | <span data-ttu-id="8c313-240">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-240">Default value</span></span> | <span data-ttu-id="8c313-241">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-241">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="8c313-242">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-242">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8c313-243">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-243">Timeout for server activity.</span></span> <span data-ttu-id="8c313-244">如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="8c313-244">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="8c313-245">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="8c313-245">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8c313-246">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="8c313-246">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="8c313-247">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-247">15 seconds</span></span> | <span data-ttu-id="8c313-248">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-248">Timeout for initial server handshake.</span></span> <span data-ttu-id="8c313-249">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="8c313-249">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="8c313-250">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-250">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8c313-251">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="8c313-251">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="8c313-252">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-252">15 seconds</span></span> | <span data-ttu-id="8c313-253">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="8c313-253">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="8c313-254">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="8c313-254">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="8c313-255">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-255">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="8c313-256">在 .NET 用戶端中,超時值`TimeSpan`指定為值。</span><span class="sxs-lookup"><span data-stu-id="8c313-256">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="8c313-257">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8c313-257">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="8c313-258">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-258">Option</span></span> | <span data-ttu-id="8c313-259">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-259">Default value</span></span> | <span data-ttu-id="8c313-260">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-260">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="8c313-261">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-261">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8c313-262">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-262">Timeout for server activity.</span></span> <span data-ttu-id="8c313-263">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="8c313-263">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="8c313-264">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="8c313-264">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8c313-265">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="8c313-265">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="8c313-266">15 秒(15,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-266">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="8c313-267">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="8c313-267">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="8c313-268">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="8c313-268">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="8c313-269">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-269">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="8c313-270">Java</span><span class="sxs-lookup"><span data-stu-id="8c313-270">Java</span></span>](#tab/java)

| <span data-ttu-id="8c313-271">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-271">Option</span></span> | <span data-ttu-id="8c313-272">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-272">Default value</span></span> | <span data-ttu-id="8c313-273">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-273">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="8c313-274">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="8c313-274">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="8c313-275">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-275">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8c313-276">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-276">Timeout for server activity.</span></span> <span data-ttu-id="8c313-277">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="8c313-277">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="8c313-278">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="8c313-278">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8c313-279">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="8c313-279">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="8c313-280">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-280">15 seconds</span></span> | <span data-ttu-id="8c313-281">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-281">Timeout for initial server handshake.</span></span> <span data-ttu-id="8c313-282">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="8c313-282">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="8c313-283">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-283">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8c313-284">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="8c313-284">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="8c313-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="8c313-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="8c313-286">15 秒(15,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-286">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="8c313-287">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="8c313-287">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="8c313-288">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="8c313-288">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="8c313-289">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-289">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="8c313-290">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="8c313-290">Configure additional options</span></span>

<span data-ttu-id="8c313-291">其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:</span><span class="sxs-lookup"><span data-stu-id="8c313-291">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="8c313-292">.NET</span><span class="sxs-lookup"><span data-stu-id="8c313-292">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="8c313-293">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="8c313-293">.NET Option</span></span> |  <span data-ttu-id="8c313-294">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-294">Default value</span></span> | <span data-ttu-id="8c313-295">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-295">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="8c313-296">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-296">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="8c313-297">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="8c313-297">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8c313-298">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="8c313-298">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8c313-299">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="8c313-299">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="8c313-300">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-300">Empty</span></span> | <span data-ttu-id="8c313-301">要發送到身份驗證請求的 TLS 證書的集合。</span><span class="sxs-lookup"><span data-stu-id="8c313-301">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="8c313-302">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-302">Empty</span></span> | <span data-ttu-id="8c313-303">要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="8c313-303">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="8c313-304">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-304">Empty</span></span> | <span data-ttu-id="8c313-305">要隨每個 HTTP 請求一起發送的認證。</span><span class="sxs-lookup"><span data-stu-id="8c313-305">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="8c313-306">5 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-306">5 seconds</span></span> | <span data-ttu-id="8c313-307">僅限 Web 插座。</span><span class="sxs-lookup"><span data-stu-id="8c313-307">WebSockets only.</span></span> <span data-ttu-id="8c313-308">用戶端關閉後等待伺服器確認關閉請求的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="8c313-308">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="8c313-309">如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-309">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="8c313-310">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-310">Empty</span></span> | <span data-ttu-id="8c313-311">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="8c313-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="8c313-312">可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-312">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="8c313-313">不用於 Web 套接字連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-313">Not used for WebSocket connections.</span></span> <span data-ttu-id="8c313-314">此委託必須返回一個非空值,並且它將作為參數接收預設值。</span><span class="sxs-lookup"><span data-stu-id="8c313-314">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="8c313-315">修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。</span><span class="sxs-lookup"><span data-stu-id="8c313-315">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="8c313-316">**替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。**</span><span class="sxs-lookup"><span data-stu-id="8c313-316">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="8c313-317">傳送 HTTP 請求時要使用的 HTTP 代理。</span><span class="sxs-lookup"><span data-stu-id="8c313-317">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="8c313-318">設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="8c313-318">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="8c313-319">這支援使用 Windows 身份驗證。</span><span class="sxs-lookup"><span data-stu-id="8c313-319">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="8c313-320">可用於配置其他 WebSocket 選項的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-320">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="8c313-321">接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。</span><span class="sxs-lookup"><span data-stu-id="8c313-321">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="8c313-322">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8c313-322">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="8c313-323">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="8c313-323">JavaScript Option</span></span> | <span data-ttu-id="8c313-324">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-324">Default Value</span></span> | <span data-ttu-id="8c313-325">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-325">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="8c313-326">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-326">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="8c313-327">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="8c313-327">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8c313-328">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="8c313-328">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8c313-329">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="8c313-329">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="8c313-330">Java</span><span class="sxs-lookup"><span data-stu-id="8c313-330">Java</span></span>](#tab/java)

| <span data-ttu-id="8c313-331">Java 選項</span><span class="sxs-lookup"><span data-stu-id="8c313-331">Java Option</span></span> | <span data-ttu-id="8c313-332">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-332">Default Value</span></span> | <span data-ttu-id="8c313-333">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-333">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="8c313-334">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-334">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="8c313-335">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="8c313-335">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8c313-336">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="8c313-336">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8c313-337">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="8c313-337">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="8c313-338">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="8c313-338">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="8c313-339">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-339">Empty</span></span> | <span data-ttu-id="8c313-340">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="8c313-340">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="8c313-341">在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:</span><span class="sxs-lookup"><span data-stu-id="8c313-341">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="8c313-342">在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:</span><span class="sxs-lookup"><span data-stu-id="8c313-342">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="8c313-343">在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="8c313-343">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="8c313-344">其他資源</span><span class="sxs-lookup"><span data-stu-id="8c313-344">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="8c313-345">JSON/訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="8c313-345">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="8c313-346">ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="8c313-346">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="8c313-347">每個協定都有序列化配置選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-347">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="8c313-348">JSON 序列化可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置,該方法`Startup.ConfigureServices`可以在方法中的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後添加。</span><span class="sxs-lookup"><span data-stu-id="8c313-348">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8c313-349">該方法`AddJsonProtocol`採用`options`接收 物件的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-349">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="8c313-350">該物件的[Payload 序列化器設定](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性`JsonSerializerSettings`是可用於 配置參數和返回值序列化 JSON.NET 物件。</span><span class="sxs-lookup"><span data-stu-id="8c313-350">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="8c313-351">有關詳細資訊,請參閱[JSON.NET 文檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="8c313-351">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="8c313-352">例如,要將序列化程式配置為使用「PascalCase」屬性名稱,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:</span><span class="sxs-lookup"><span data-stu-id="8c313-352">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="8c313-353">在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="8c313-353">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="8c313-354">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:</span><span class="sxs-lookup"><span data-stu-id="8c313-354">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="8c313-355">此時無法在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="8c313-355">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="8c313-356">訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="8c313-356">MessagePack serialization options</span></span>

<span data-ttu-id="8c313-357">消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。</span><span class="sxs-lookup"><span data-stu-id="8c313-357">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="8c313-358">有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。</span><span class="sxs-lookup"><span data-stu-id="8c313-358">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="8c313-359">此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="8c313-359">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="8c313-360">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="8c313-360">Configure server options</span></span>

<span data-ttu-id="8c313-361">下表描述了用於設定 SignalR 集線器的選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-361">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="8c313-362">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-362">Option</span></span> | <span data-ttu-id="8c313-363">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-363">Default Value</span></span> | <span data-ttu-id="8c313-364">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-364">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="8c313-365">30 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-365">30 seconds</span></span> | <span data-ttu-id="8c313-366">如果用戶端未在此間隔內收到消息(包括保持活動狀態),伺服器將考慮用戶端斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-366">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="8c313-367">由於如何實現,實際將客戶端標記為斷開連接的時間可能比此超時間隔長。</span><span class="sxs-lookup"><span data-stu-id="8c313-367">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="8c313-368">建議的值是值的`KeepAliveInterval`兩倍。</span><span class="sxs-lookup"><span data-stu-id="8c313-368">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="8c313-369">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-369">15 seconds</span></span> | <span data-ttu-id="8c313-370">如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。</span><span class="sxs-lookup"><span data-stu-id="8c313-370">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="8c313-371">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-371">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8c313-372">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="8c313-372">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="8c313-373">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-373">15 seconds</span></span> | <span data-ttu-id="8c313-374">如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。</span><span class="sxs-lookup"><span data-stu-id="8c313-374">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="8c313-375">更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-375">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="8c313-376">建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /</span><span class="sxs-lookup"><span data-stu-id="8c313-376">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="8c313-377">所有已安裝的協定</span><span class="sxs-lookup"><span data-stu-id="8c313-377">All installed protocols</span></span> | <span data-ttu-id="8c313-378">此中心支持的協定。</span><span class="sxs-lookup"><span data-stu-id="8c313-378">Protocols supported by this hub.</span></span> <span data-ttu-id="8c313-379">默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。</span><span class="sxs-lookup"><span data-stu-id="8c313-379">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="8c313-380">如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。</span><span class="sxs-lookup"><span data-stu-id="8c313-380">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="8c313-381">默認值為`false`,因為這些異常消息可能包含敏感資訊。</span><span class="sxs-lookup"><span data-stu-id="8c313-381">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="8c313-382">可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-382">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="8c313-383">單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :</span><span class="sxs-lookup"><span data-stu-id="8c313-383">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="8c313-384">進階 HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="8c313-384">Advanced HTTP configuration options</span></span>

<span data-ttu-id="8c313-385">用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-385">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="8c313-386">這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)在`Startup.Configure`中配置。</span><span class="sxs-lookup"><span data-stu-id="8c313-386">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="8c313-387">下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-387">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="8c313-388">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-388">Option</span></span> | <span data-ttu-id="8c313-389">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-389">Default Value</span></span> | <span data-ttu-id="8c313-390">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-390">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="8c313-391">32 KB</span><span class="sxs-lookup"><span data-stu-id="8c313-391">32 KB</span></span> | <span data-ttu-id="8c313-392">從伺服器緩衝的用戶端接收的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="8c313-392">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="8c313-393">增加此值允許伺服器接收較大的消息,但可能會對記憶體消耗產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="8c313-393">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="8c313-394">數據自動從應用於中心`Authorize`類的屬性中收集。</span><span class="sxs-lookup"><span data-stu-id="8c313-394">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="8c313-395">用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。</span><span class="sxs-lookup"><span data-stu-id="8c313-395">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="8c313-396">32 KB</span><span class="sxs-lookup"><span data-stu-id="8c313-396">32 KB</span></span> | <span data-ttu-id="8c313-397">伺服器緩衝的應用發送的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="8c313-397">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="8c313-398">增加此值允許伺服器發送更大的消息,但可能會對記憶體消耗產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="8c313-398">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="8c313-399">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="8c313-399">All Transports are enabled.</span></span> | <span data-ttu-id="8c313-400">位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="8c313-400">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="8c313-401">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="8c313-401">See below.</span></span> | <span data-ttu-id="8c313-402">特定於長輪詢傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-402">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="8c313-403">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="8c313-403">See below.</span></span> | <span data-ttu-id="8c313-404">特定於 WebSocket 傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-404">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="8c313-405">長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-405">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="8c313-406">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-406">Option</span></span> | <span data-ttu-id="8c313-407">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-407">Default Value</span></span> | <span data-ttu-id="8c313-408">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-408">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="8c313-409">90 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-409">90 seconds</span></span> | <span data-ttu-id="8c313-410">伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。</span><span class="sxs-lookup"><span data-stu-id="8c313-410">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="8c313-411">減小此值會導致用戶端更頻繁地發出新的輪詢請求。</span><span class="sxs-lookup"><span data-stu-id="8c313-411">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="8c313-412">WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-412">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="8c313-413">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-413">Option</span></span> | <span data-ttu-id="8c313-414">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-414">Default Value</span></span> | <span data-ttu-id="8c313-415">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-415">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="8c313-416">5 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-416">5 seconds</span></span> | <span data-ttu-id="8c313-417">伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。</span><span class="sxs-lookup"><span data-stu-id="8c313-417">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="8c313-418">可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-418">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="8c313-419">委託接收用戶端請求的值作為輸入,並預期返回所需的值。</span><span class="sxs-lookup"><span data-stu-id="8c313-419">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="8c313-420">設定客戶端選項</span><span class="sxs-lookup"><span data-stu-id="8c313-420">Configure client options</span></span>

<span data-ttu-id="8c313-421">可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。</span><span class="sxs-lookup"><span data-stu-id="8c313-421">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="8c313-422">它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。</span><span class="sxs-lookup"><span data-stu-id="8c313-422">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="8c313-423">設定記錄</span><span class="sxs-lookup"><span data-stu-id="8c313-423">Configure logging</span></span>

<span data-ttu-id="8c313-424">使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="8c313-424">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="8c313-425">日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。</span><span class="sxs-lookup"><span data-stu-id="8c313-425">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="8c313-426">有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。</span><span class="sxs-lookup"><span data-stu-id="8c313-426">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="8c313-427">要註冊日誌記錄提供程式,必須安裝必要的包。</span><span class="sxs-lookup"><span data-stu-id="8c313-427">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="8c313-428">有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。</span><span class="sxs-lookup"><span data-stu-id="8c313-428">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="8c313-429">例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="8c313-429">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="8c313-430">呼叫`AddConsole`式副用:</span><span class="sxs-lookup"><span data-stu-id="8c313-430">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="8c313-431">在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。</span><span class="sxs-lookup"><span data-stu-id="8c313-431">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="8c313-432">提供指示`LogLevel`要生產的日誌消息的最小級別的值。</span><span class="sxs-lookup"><span data-stu-id="8c313-432">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="8c313-433">日誌將寫入瀏覽器控制台視窗。</span><span class="sxs-lookup"><span data-stu-id="8c313-433">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="8c313-434">要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。</span><span class="sxs-lookup"><span data-stu-id="8c313-434">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="8c313-435">關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="8c313-435">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="8c313-436">SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="8c313-436">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="8c313-437">它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。</span><span class="sxs-lookup"><span data-stu-id="8c313-437">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="8c313-438">以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。</span><span class="sxs-lookup"><span data-stu-id="8c313-438">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="8c313-439">如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:</span><span class="sxs-lookup"><span data-stu-id="8c313-439">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="8c313-440">這可以安全地忽略。</span><span class="sxs-lookup"><span data-stu-id="8c313-440">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="8c313-441">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="8c313-441">Configure allowed transports</span></span>

<span data-ttu-id="8c313-442">SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。</span><span class="sxs-lookup"><span data-stu-id="8c313-442">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="8c313-443">值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="8c313-443">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="8c313-444">默認情況下,所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="8c313-444">All transports are enabled by default.</span></span>

<span data-ttu-id="8c313-445">例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:</span><span class="sxs-lookup"><span data-stu-id="8c313-445">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="8c313-446">在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:</span><span class="sxs-lookup"><span data-stu-id="8c313-446">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="8c313-447">在此版本中,JAva 用戶端 Websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="8c313-447">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="8c313-448">設定無記名身份驗證</span><span class="sxs-lookup"><span data-stu-id="8c313-448">Configure bearer authentication</span></span>

<span data-ttu-id="8c313-449">要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-449">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="8c313-450">在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。</span><span class="sxs-lookup"><span data-stu-id="8c313-450">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="8c313-451">在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。</span><span class="sxs-lookup"><span data-stu-id="8c313-451">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="8c313-452">在這些情況下,訪問權杖作為查詢字串值`access_token`提供。</span><span class="sxs-lookup"><span data-stu-id="8c313-452">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="8c313-453">在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-453">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="8c313-454">在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="8c313-454">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="8c313-455">在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。</span><span class="sxs-lookup"><span data-stu-id="8c313-455">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="8c313-456">[與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html)</span><span class="sxs-lookup"><span data-stu-id="8c313-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="8c313-457">調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="8c313-457">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="8c313-458">設定逾時與保持作用選項</span><span class="sxs-lookup"><span data-stu-id="8c313-458">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="8c313-459">`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:</span><span class="sxs-lookup"><span data-stu-id="8c313-459">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="8c313-460">.NET</span><span class="sxs-lookup"><span data-stu-id="8c313-460">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="8c313-461">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-461">Option</span></span> | <span data-ttu-id="8c313-462">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-462">Default value</span></span> | <span data-ttu-id="8c313-463">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-463">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="8c313-464">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-464">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8c313-465">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-465">Timeout for server activity.</span></span> <span data-ttu-id="8c313-466">如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="8c313-466">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="8c313-467">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="8c313-467">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8c313-468">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="8c313-468">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="8c313-469">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-469">15 seconds</span></span> | <span data-ttu-id="8c313-470">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-470">Timeout for initial server handshake.</span></span> <span data-ttu-id="8c313-471">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="8c313-471">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="8c313-472">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-472">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8c313-473">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="8c313-473">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="8c313-474">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-474">15 seconds</span></span> | <span data-ttu-id="8c313-475">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="8c313-475">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="8c313-476">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="8c313-476">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="8c313-477">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-477">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="8c313-478">在 .NET 用戶端中,超時值`TimeSpan`指定為值。</span><span class="sxs-lookup"><span data-stu-id="8c313-478">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="8c313-479">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8c313-479">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="8c313-480">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-480">Option</span></span> | <span data-ttu-id="8c313-481">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-481">Default value</span></span> | <span data-ttu-id="8c313-482">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-482">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="8c313-483">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-483">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8c313-484">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-484">Timeout for server activity.</span></span> <span data-ttu-id="8c313-485">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="8c313-485">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="8c313-486">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="8c313-486">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8c313-487">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="8c313-487">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="8c313-488">15 秒(15,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-488">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="8c313-489">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="8c313-489">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="8c313-490">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="8c313-490">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="8c313-491">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-491">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="8c313-492">Java</span><span class="sxs-lookup"><span data-stu-id="8c313-492">Java</span></span>](#tab/java)

| <span data-ttu-id="8c313-493">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-493">Option</span></span> | <span data-ttu-id="8c313-494">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-494">Default value</span></span> | <span data-ttu-id="8c313-495">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-495">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="8c313-496">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="8c313-496">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="8c313-497">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-497">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8c313-498">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-498">Timeout for server activity.</span></span> <span data-ttu-id="8c313-499">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="8c313-499">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="8c313-500">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="8c313-500">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8c313-501">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="8c313-501">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="8c313-502">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-502">15 seconds</span></span> | <span data-ttu-id="8c313-503">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-503">Timeout for initial server handshake.</span></span> <span data-ttu-id="8c313-504">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="8c313-504">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="8c313-505">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-505">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8c313-506">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="8c313-506">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="8c313-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="8c313-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="8c313-508">15 秒(15,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-508">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="8c313-509">確定客戶端發送 ping 消息的時間間隔。</span><span class="sxs-lookup"><span data-stu-id="8c313-509">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="8c313-510">從用戶端發送任何消息會將計時器重置為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="8c313-510">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="8c313-511">如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-511">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="8c313-512">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="8c313-512">Configure additional options</span></span>

<span data-ttu-id="8c313-513">其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:</span><span class="sxs-lookup"><span data-stu-id="8c313-513">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="8c313-514">.NET</span><span class="sxs-lookup"><span data-stu-id="8c313-514">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="8c313-515">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="8c313-515">.NET Option</span></span> |  <span data-ttu-id="8c313-516">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-516">Default value</span></span> | <span data-ttu-id="8c313-517">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-517">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="8c313-518">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-518">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="8c313-519">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="8c313-519">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8c313-520">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="8c313-520">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8c313-521">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="8c313-521">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="8c313-522">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-522">Empty</span></span> | <span data-ttu-id="8c313-523">要發送到身份驗證請求的 TLS 證書的集合。</span><span class="sxs-lookup"><span data-stu-id="8c313-523">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="8c313-524">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-524">Empty</span></span> | <span data-ttu-id="8c313-525">要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="8c313-525">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="8c313-526">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-526">Empty</span></span> | <span data-ttu-id="8c313-527">要隨每個 HTTP 請求一起發送的認證。</span><span class="sxs-lookup"><span data-stu-id="8c313-527">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="8c313-528">5 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-528">5 seconds</span></span> | <span data-ttu-id="8c313-529">僅限 Web 插座。</span><span class="sxs-lookup"><span data-stu-id="8c313-529">WebSockets only.</span></span> <span data-ttu-id="8c313-530">用戶端關閉後等待伺服器確認關閉請求的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="8c313-530">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="8c313-531">如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-531">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="8c313-532">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-532">Empty</span></span> | <span data-ttu-id="8c313-533">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="8c313-533">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="8c313-534">可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-534">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="8c313-535">不用於 Web 套接字連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-535">Not used for WebSocket connections.</span></span> <span data-ttu-id="8c313-536">此委託必須返回一個非空值,並且它將作為參數接收預設值。</span><span class="sxs-lookup"><span data-stu-id="8c313-536">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="8c313-537">修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。</span><span class="sxs-lookup"><span data-stu-id="8c313-537">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="8c313-538">**替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。**</span><span class="sxs-lookup"><span data-stu-id="8c313-538">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="8c313-539">傳送 HTTP 請求時要使用的 HTTP 代理。</span><span class="sxs-lookup"><span data-stu-id="8c313-539">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="8c313-540">設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="8c313-540">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="8c313-541">這支援使用 Windows 身份驗證。</span><span class="sxs-lookup"><span data-stu-id="8c313-541">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="8c313-542">可用於配置其他 WebSocket 選項的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-542">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="8c313-543">接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。</span><span class="sxs-lookup"><span data-stu-id="8c313-543">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="8c313-544">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8c313-544">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="8c313-545">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="8c313-545">JavaScript Option</span></span> | <span data-ttu-id="8c313-546">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-546">Default Value</span></span> | <span data-ttu-id="8c313-547">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-547">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="8c313-548">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-548">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="8c313-549">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="8c313-549">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8c313-550">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="8c313-550">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8c313-551">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="8c313-551">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="8c313-552">Java</span><span class="sxs-lookup"><span data-stu-id="8c313-552">Java</span></span>](#tab/java)

| <span data-ttu-id="8c313-553">Java 選項</span><span class="sxs-lookup"><span data-stu-id="8c313-553">Java Option</span></span> | <span data-ttu-id="8c313-554">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-554">Default Value</span></span> | <span data-ttu-id="8c313-555">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-555">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="8c313-556">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-556">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="8c313-557">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="8c313-557">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8c313-558">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="8c313-558">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8c313-559">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="8c313-559">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="8c313-560">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="8c313-560">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="8c313-561">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-561">Empty</span></span> | <span data-ttu-id="8c313-562">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="8c313-562">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="8c313-563">在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:</span><span class="sxs-lookup"><span data-stu-id="8c313-563">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="8c313-564">在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:</span><span class="sxs-lookup"><span data-stu-id="8c313-564">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="8c313-565">在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="8c313-565">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="8c313-566">其他資源</span><span class="sxs-lookup"><span data-stu-id="8c313-566">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="8c313-567">JSON/訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="8c313-567">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="8c313-568">ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="8c313-568">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="8c313-569">每個協定都有序列化配置選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-569">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="8c313-570">JSON 序列化可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置,該方法`Startup.ConfigureServices`可以在方法中的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後添加。</span><span class="sxs-lookup"><span data-stu-id="8c313-570">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8c313-571">該方法`AddJsonProtocol`採用`options`接收 物件的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-571">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="8c313-572">該物件的[Payload 序列化器設定](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性`JsonSerializerSettings`是可用於 配置參數和返回值序列化 JSON.NET 物件。</span><span class="sxs-lookup"><span data-stu-id="8c313-572">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="8c313-573">有關詳細資訊,請參閱[JSON.NET 文檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="8c313-573">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="8c313-574">例如,要將序列化程式配置為使用「PascalCase」屬性名稱,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:</span><span class="sxs-lookup"><span data-stu-id="8c313-574">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="8c313-575">在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="8c313-575">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="8c313-576">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:</span><span class="sxs-lookup"><span data-stu-id="8c313-576">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="8c313-577">此時無法在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="8c313-577">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="8c313-578">訊息套件序列化選項</span><span class="sxs-lookup"><span data-stu-id="8c313-578">MessagePack serialization options</span></span>

<span data-ttu-id="8c313-579">消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。</span><span class="sxs-lookup"><span data-stu-id="8c313-579">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="8c313-580">有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。</span><span class="sxs-lookup"><span data-stu-id="8c313-580">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="8c313-581">此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="8c313-581">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="8c313-582">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="8c313-582">Configure server options</span></span>

<span data-ttu-id="8c313-583">下表描述了用於設定 SignalR 集線器的選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-583">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="8c313-584">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-584">Option</span></span> | <span data-ttu-id="8c313-585">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-585">Default Value</span></span> | <span data-ttu-id="8c313-586">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-586">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="8c313-587">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-587">15 seconds</span></span> | <span data-ttu-id="8c313-588">如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。</span><span class="sxs-lookup"><span data-stu-id="8c313-588">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="8c313-589">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-589">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8c313-590">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="8c313-590">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="8c313-591">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-591">15 seconds</span></span> | <span data-ttu-id="8c313-592">如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。</span><span class="sxs-lookup"><span data-stu-id="8c313-592">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="8c313-593">更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-593">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="8c313-594">建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /</span><span class="sxs-lookup"><span data-stu-id="8c313-594">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="8c313-595">所有已安裝的協定</span><span class="sxs-lookup"><span data-stu-id="8c313-595">All installed protocols</span></span> | <span data-ttu-id="8c313-596">此中心支持的協定。</span><span class="sxs-lookup"><span data-stu-id="8c313-596">Protocols supported by this hub.</span></span> <span data-ttu-id="8c313-597">默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。</span><span class="sxs-lookup"><span data-stu-id="8c313-597">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="8c313-598">如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。</span><span class="sxs-lookup"><span data-stu-id="8c313-598">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="8c313-599">默認值為`false`,因為這些異常消息可能包含敏感資訊。</span><span class="sxs-lookup"><span data-stu-id="8c313-599">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="8c313-600">可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-600">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="8c313-601">單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :</span><span class="sxs-lookup"><span data-stu-id="8c313-601">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="8c313-602">進階 HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="8c313-602">Advanced HTTP configuration options</span></span>

<span data-ttu-id="8c313-603">用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-603">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="8c313-604">這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)在`Startup.Configure`中配置。</span><span class="sxs-lookup"><span data-stu-id="8c313-604">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="8c313-605">下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-605">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="8c313-606">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-606">Option</span></span> | <span data-ttu-id="8c313-607">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-607">Default Value</span></span> | <span data-ttu-id="8c313-608">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="8c313-609">32 KB</span><span class="sxs-lookup"><span data-stu-id="8c313-609">32 KB</span></span> | <span data-ttu-id="8c313-610">從伺服器緩衝的用戶端接收的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="8c313-610">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="8c313-611">增加此值允許伺服器接收較大的消息,但可能會對記憶體消耗產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="8c313-611">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="8c313-612">數據自動從應用於中心`Authorize`類的屬性中收集。</span><span class="sxs-lookup"><span data-stu-id="8c313-612">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="8c313-613">用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。</span><span class="sxs-lookup"><span data-stu-id="8c313-613">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="8c313-614">32 KB</span><span class="sxs-lookup"><span data-stu-id="8c313-614">32 KB</span></span> | <span data-ttu-id="8c313-615">伺服器緩衝的應用發送的最大位元組數。</span><span class="sxs-lookup"><span data-stu-id="8c313-615">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="8c313-616">增加此值允許伺服器發送更大的消息,但可能會對記憶體消耗產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="8c313-616">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="8c313-617">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="8c313-617">All Transports are enabled.</span></span> | <span data-ttu-id="8c313-618">位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="8c313-618">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="8c313-619">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="8c313-619">See below.</span></span> | <span data-ttu-id="8c313-620">特定於長輪詢傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-620">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="8c313-621">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="8c313-621">See below.</span></span> | <span data-ttu-id="8c313-622">特定於 WebSocket 傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="8c313-622">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="8c313-623">長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-623">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="8c313-624">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-624">Option</span></span> | <span data-ttu-id="8c313-625">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-625">Default Value</span></span> | <span data-ttu-id="8c313-626">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-626">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="8c313-627">90 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-627">90 seconds</span></span> | <span data-ttu-id="8c313-628">伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。</span><span class="sxs-lookup"><span data-stu-id="8c313-628">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="8c313-629">減小此值會導致用戶端更頻繁地發出新的輪詢請求。</span><span class="sxs-lookup"><span data-stu-id="8c313-629">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="8c313-630">WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-630">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="8c313-631">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-631">Option</span></span> | <span data-ttu-id="8c313-632">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-632">Default Value</span></span> | <span data-ttu-id="8c313-633">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-633">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="8c313-634">5 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-634">5 seconds</span></span> | <span data-ttu-id="8c313-635">伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。</span><span class="sxs-lookup"><span data-stu-id="8c313-635">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="8c313-636">可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-636">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="8c313-637">委託接收用戶端請求的值作為輸入,並預期返回所需的值。</span><span class="sxs-lookup"><span data-stu-id="8c313-637">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="8c313-638">設定客戶端選項</span><span class="sxs-lookup"><span data-stu-id="8c313-638">Configure client options</span></span>

<span data-ttu-id="8c313-639">可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。</span><span class="sxs-lookup"><span data-stu-id="8c313-639">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="8c313-640">它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。</span><span class="sxs-lookup"><span data-stu-id="8c313-640">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="8c313-641">設定記錄</span><span class="sxs-lookup"><span data-stu-id="8c313-641">Configure logging</span></span>

<span data-ttu-id="8c313-642">使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="8c313-642">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="8c313-643">日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。</span><span class="sxs-lookup"><span data-stu-id="8c313-643">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="8c313-644">有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。</span><span class="sxs-lookup"><span data-stu-id="8c313-644">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="8c313-645">要註冊日誌記錄提供程式,必須安裝必要的包。</span><span class="sxs-lookup"><span data-stu-id="8c313-645">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="8c313-646">有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。</span><span class="sxs-lookup"><span data-stu-id="8c313-646">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="8c313-647">例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="8c313-647">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="8c313-648">呼叫`AddConsole`式副用:</span><span class="sxs-lookup"><span data-stu-id="8c313-648">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="8c313-649">在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。</span><span class="sxs-lookup"><span data-stu-id="8c313-649">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="8c313-650">提供指示`LogLevel`要生產的日誌消息的最小級別的值。</span><span class="sxs-lookup"><span data-stu-id="8c313-650">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="8c313-651">日誌將寫入瀏覽器控制台視窗。</span><span class="sxs-lookup"><span data-stu-id="8c313-651">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="8c313-652">要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。</span><span class="sxs-lookup"><span data-stu-id="8c313-652">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="8c313-653">關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="8c313-653">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="8c313-654">SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="8c313-654">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="8c313-655">它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。</span><span class="sxs-lookup"><span data-stu-id="8c313-655">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="8c313-656">以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。</span><span class="sxs-lookup"><span data-stu-id="8c313-656">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="8c313-657">如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:</span><span class="sxs-lookup"><span data-stu-id="8c313-657">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="8c313-658">這可以安全地忽略。</span><span class="sxs-lookup"><span data-stu-id="8c313-658">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="8c313-659">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="8c313-659">Configure allowed transports</span></span>

<span data-ttu-id="8c313-660">SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。</span><span class="sxs-lookup"><span data-stu-id="8c313-660">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="8c313-661">值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="8c313-661">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="8c313-662">默認情況下,所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="8c313-662">All transports are enabled by default.</span></span>

<span data-ttu-id="8c313-663">例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:</span><span class="sxs-lookup"><span data-stu-id="8c313-663">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="8c313-664">在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:</span><span class="sxs-lookup"><span data-stu-id="8c313-664">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="8c313-665">設定無記名身份驗證</span><span class="sxs-lookup"><span data-stu-id="8c313-665">Configure bearer authentication</span></span>

<span data-ttu-id="8c313-666">要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-666">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="8c313-667">在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。</span><span class="sxs-lookup"><span data-stu-id="8c313-667">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="8c313-668">在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。</span><span class="sxs-lookup"><span data-stu-id="8c313-668">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="8c313-669">在這些情況下,訪問權杖作為查詢字串值`access_token`提供。</span><span class="sxs-lookup"><span data-stu-id="8c313-669">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="8c313-670">在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:</span><span class="sxs-lookup"><span data-stu-id="8c313-670">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="8c313-671">在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="8c313-671">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="8c313-672">在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。</span><span class="sxs-lookup"><span data-stu-id="8c313-672">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="8c313-673">[與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html)</span><span class="sxs-lookup"><span data-stu-id="8c313-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="8c313-674">調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="8c313-674">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="8c313-675">設定逾時與保持作用選項</span><span class="sxs-lookup"><span data-stu-id="8c313-675">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="8c313-676">`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:</span><span class="sxs-lookup"><span data-stu-id="8c313-676">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="8c313-677">.NET</span><span class="sxs-lookup"><span data-stu-id="8c313-677">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="8c313-678">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-678">Option</span></span> | <span data-ttu-id="8c313-679">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-679">Default value</span></span> | <span data-ttu-id="8c313-680">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-680">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="8c313-681">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-681">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8c313-682">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-682">Timeout for server activity.</span></span> <span data-ttu-id="8c313-683">如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="8c313-683">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="8c313-684">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="8c313-684">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8c313-685">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="8c313-685">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="8c313-686">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-686">15 seconds</span></span> | <span data-ttu-id="8c313-687">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-687">Timeout for initial server handshake.</span></span> <span data-ttu-id="8c313-688">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="8c313-688">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="8c313-689">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-689">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8c313-690">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="8c313-690">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="8c313-691">在 .NET 用戶端中,超時值`TimeSpan`指定為值。</span><span class="sxs-lookup"><span data-stu-id="8c313-691">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="8c313-692">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8c313-692">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="8c313-693">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-693">Option</span></span> | <span data-ttu-id="8c313-694">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-694">Default value</span></span> | <span data-ttu-id="8c313-695">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-695">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="8c313-696">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-696">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8c313-697">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-697">Timeout for server activity.</span></span> <span data-ttu-id="8c313-698">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="8c313-698">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="8c313-699">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="8c313-699">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8c313-700">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="8c313-700">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="java"></a>[<span data-ttu-id="8c313-701">Java</span><span class="sxs-lookup"><span data-stu-id="8c313-701">Java</span></span>](#tab/java)

| <span data-ttu-id="8c313-702">選項</span><span class="sxs-lookup"><span data-stu-id="8c313-702">Option</span></span> | <span data-ttu-id="8c313-703">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-703">Default value</span></span> | <span data-ttu-id="8c313-704">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-704">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="8c313-705">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="8c313-705">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="8c313-706">30 秒(30,000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="8c313-706">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8c313-707">伺服器活動的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-707">Timeout for server activity.</span></span> <span data-ttu-id="8c313-708">如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。</span><span class="sxs-lookup"><span data-stu-id="8c313-708">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="8c313-709">此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。</span><span class="sxs-lookup"><span data-stu-id="8c313-709">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8c313-710">建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。</span><span class="sxs-lookup"><span data-stu-id="8c313-710">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="8c313-711">15 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-711">15 seconds</span></span> | <span data-ttu-id="8c313-712">初始伺服器握手的超時。</span><span class="sxs-lookup"><span data-stu-id="8c313-712">Timeout for initial server handshake.</span></span> <span data-ttu-id="8c313-713">如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="8c313-713">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="8c313-714">這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。</span><span class="sxs-lookup"><span data-stu-id="8c313-714">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8c313-715">有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="8c313-715">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="8c313-716">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="8c313-716">Configure additional options</span></span>

<span data-ttu-id="8c313-717">其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:</span><span class="sxs-lookup"><span data-stu-id="8c313-717">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="8c313-718">.NET</span><span class="sxs-lookup"><span data-stu-id="8c313-718">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="8c313-719">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="8c313-719">.NET Option</span></span> |  <span data-ttu-id="8c313-720">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-720">Default value</span></span> | <span data-ttu-id="8c313-721">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-721">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="8c313-722">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-722">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="8c313-723">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="8c313-723">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8c313-724">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="8c313-724">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8c313-725">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="8c313-725">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="8c313-726">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-726">Empty</span></span> | <span data-ttu-id="8c313-727">要發送到身份驗證請求的 TLS 證書的集合。</span><span class="sxs-lookup"><span data-stu-id="8c313-727">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="8c313-728">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-728">Empty</span></span> | <span data-ttu-id="8c313-729">要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="8c313-729">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="8c313-730">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-730">Empty</span></span> | <span data-ttu-id="8c313-731">要隨每個 HTTP 請求一起發送的認證。</span><span class="sxs-lookup"><span data-stu-id="8c313-731">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="8c313-732">5 秒</span><span class="sxs-lookup"><span data-stu-id="8c313-732">5 seconds</span></span> | <span data-ttu-id="8c313-733">僅限 Web 插座。</span><span class="sxs-lookup"><span data-stu-id="8c313-733">WebSockets only.</span></span> <span data-ttu-id="8c313-734">用戶端關閉後等待伺服器確認關閉請求的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="8c313-734">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="8c313-735">如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-735">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="8c313-736">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-736">Empty</span></span> | <span data-ttu-id="8c313-737">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="8c313-737">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="8c313-738">可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-738">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="8c313-739">不用於 Web 套接字連接。</span><span class="sxs-lookup"><span data-stu-id="8c313-739">Not used for WebSocket connections.</span></span> <span data-ttu-id="8c313-740">此委託必須返回一個非空值,並且它將作為參數接收預設值。</span><span class="sxs-lookup"><span data-stu-id="8c313-740">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="8c313-741">修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。</span><span class="sxs-lookup"><span data-stu-id="8c313-741">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="8c313-742">**替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。**</span><span class="sxs-lookup"><span data-stu-id="8c313-742">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="8c313-743">傳送 HTTP 請求時要使用的 HTTP 代理。</span><span class="sxs-lookup"><span data-stu-id="8c313-743">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="8c313-744">設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="8c313-744">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="8c313-745">這支援使用 Windows 身份驗證。</span><span class="sxs-lookup"><span data-stu-id="8c313-745">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="8c313-746">可用於配置其他 WebSocket 選項的委託。</span><span class="sxs-lookup"><span data-stu-id="8c313-746">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="8c313-747">接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。</span><span class="sxs-lookup"><span data-stu-id="8c313-747">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="8c313-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8c313-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="8c313-749">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="8c313-749">JavaScript Option</span></span> | <span data-ttu-id="8c313-750">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-750">Default Value</span></span> | <span data-ttu-id="8c313-751">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-751">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="8c313-752">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-752">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="8c313-753">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="8c313-753">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8c313-754">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="8c313-754">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8c313-755">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="8c313-755">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="8c313-756">Java</span><span class="sxs-lookup"><span data-stu-id="8c313-756">Java</span></span>](#tab/java)

| <span data-ttu-id="8c313-757">Java 選項</span><span class="sxs-lookup"><span data-stu-id="8c313-757">Java Option</span></span> | <span data-ttu-id="8c313-758">預設值</span><span class="sxs-lookup"><span data-stu-id="8c313-758">Default Value</span></span> | <span data-ttu-id="8c313-759">描述</span><span class="sxs-lookup"><span data-stu-id="8c313-759">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="8c313-760">返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。</span><span class="sxs-lookup"><span data-stu-id="8c313-760">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="8c313-761">將此`true`設置為跳過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="8c313-761">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8c313-762">**只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。</span><span class="sxs-lookup"><span data-stu-id="8c313-762">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8c313-763">使用 Azure SignalR 服務時無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="8c313-763">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="8c313-764">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="8c313-764">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="8c313-765">空白</span><span class="sxs-lookup"><span data-stu-id="8c313-765">Empty</span></span> | <span data-ttu-id="8c313-766">要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。</span><span class="sxs-lookup"><span data-stu-id="8c313-766">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="8c313-767">在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:</span><span class="sxs-lookup"><span data-stu-id="8c313-767">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="8c313-768">在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:</span><span class="sxs-lookup"><span data-stu-id="8c313-768">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="8c313-769">在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="8c313-769">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="8c313-770">其他資源</span><span class="sxs-lookup"><span data-stu-id="8c313-770">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
