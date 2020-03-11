---
title: ASP.NET Core SignalR 設定
author: bradygaster
description: 瞭解如何設定 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/10/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: c225ff88110dc17185a430ac1c422d2433306115
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658631"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="91663-103">ASP.NET Core SignalR 設定</span><span class="sxs-lookup"><span data-stu-id="91663-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="91663-104">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="91663-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="91663-105">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="91663-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="91663-106">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="91663-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="91663-107">您可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，在伺服器上設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="91663-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="91663-108">`AddJsonProtocol` 可以在 `Startup.ConfigureServices`中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="91663-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="91663-109">`AddJsonProtocol` 方法會接受一個可接收 `options` 物件的委派。</span><span class="sxs-lookup"><span data-stu-id="91663-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="91663-110">該物件上的[PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是 `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> 物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="91663-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="91663-111">如需詳細資訊，請參閱[system.web 檔](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="91663-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="91663-112">例如，若要設定序列化程式不變更屬性名稱的大小寫，而不是預設的 "camelCase" 名稱，請在 `Startup.ConfigureServices`中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="91663-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="91663-113">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="91663-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="91663-114">必須匯入 `Microsoft.Extensions.DependencyInjection` 命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="91663-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="91663-115">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="91663-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="91663-116">切換至 Newtonsoft. Json</span><span class="sxs-lookup"><span data-stu-id="91663-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="91663-117">如果您需要 `System.Text.Json`中不支援的 `Newtonsoft.Json` 功能，請參閱[切換至 Newtonsoft。](xref:migration/22-to-30#switch-to-newtonsoftjson)</span><span class="sxs-lookup"><span data-stu-id="91663-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="91663-118">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="91663-118">MessagePack serialization options</span></span>

<span data-ttu-id="91663-119">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="91663-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="91663-120">如需詳細資訊，請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="91663-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="91663-121">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="91663-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="91663-122">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="91663-122">Configure server options</span></span>

<span data-ttu-id="91663-123">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="91663-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="91663-124">選項</span><span class="sxs-lookup"><span data-stu-id="91663-124">Option</span></span> | <span data-ttu-id="91663-125">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-125">Default Value</span></span> | <span data-ttu-id="91663-126">描述</span><span class="sxs-lookup"><span data-stu-id="91663-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="91663-127">30 秒</span><span class="sxs-lookup"><span data-stu-id="91663-127">30 seconds</span></span> | <span data-ttu-id="91663-128">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="91663-129">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="91663-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="91663-130">建議的值是 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="91663-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="91663-131">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-131">15 seconds</span></span> | <span data-ttu-id="91663-132">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="91663-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="91663-133">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="91663-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="91663-134">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="91663-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="91663-135">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-135">15 seconds</span></span> | <span data-ttu-id="91663-136">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="91663-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="91663-137">當變更 `KeepAliveInterval`時，請變更用戶端上的 `ServerTimeout`/`serverTimeoutInMilliseconds` 設定。</span><span class="sxs-lookup"><span data-stu-id="91663-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="91663-138">建議的 `ServerTimeout`/`serverTimeoutInMilliseconds` 值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="91663-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="91663-139">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="91663-139">All installed protocols</span></span> | <span data-ttu-id="91663-140">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="91663-140">Protocols supported by this hub.</span></span> <span data-ttu-id="91663-141">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="91663-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="91663-142">如果 `true`，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="91663-143">預設值為 `false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="91663-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="91663-144">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="91663-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="91663-145">若達到此限制，則在伺服器處理資料流程專案之前，會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="91663-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="91663-146">32 KB</span><span class="sxs-lookup"><span data-stu-id="91663-146">32 KB</span></span> | <span data-ttu-id="91663-147">單一傳入中樞訊息的大小上限。</span><span class="sxs-lookup"><span data-stu-id="91663-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="91663-148">您可以為所有中樞設定選項，方法是在 `Startup.ConfigureServices`中提供 `AddSignalR` 呼叫的選項委派。</span><span class="sxs-lookup"><span data-stu-id="91663-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="91663-149">單一中樞的選項會覆寫 `AddSignalR` 中提供的全域選項，而且可以使用 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="91663-150">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="91663-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="91663-151">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="91663-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="91663-152">這些選項是藉由將委派傳遞至 `Startup.Configure`中的[MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub)來設定。</span><span class="sxs-lookup"><span data-stu-id="91663-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="91663-153">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="91663-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="91663-154">選項</span><span class="sxs-lookup"><span data-stu-id="91663-154">Option</span></span> | <span data-ttu-id="91663-155">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-155">Default Value</span></span> | <span data-ttu-id="91663-156">描述</span><span class="sxs-lookup"><span data-stu-id="91663-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="91663-157">32 KB</span><span class="sxs-lookup"><span data-stu-id="91663-157">32 KB</span></span> | <span data-ttu-id="91663-158">在套用背壓之前，伺服器會緩衝的用戶端接收的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="91663-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="91663-159">增加此值可讓伺服器更快速地接收較大的訊息，而不需套用背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="91663-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="91663-160">自動從套用至中樞類別的 `Authorize` 屬性收集資料。</span><span class="sxs-lookup"><span data-stu-id="91663-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="91663-161">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="91663-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="91663-162">32 KB</span><span class="sxs-lookup"><span data-stu-id="91663-162">32 KB</span></span> | <span data-ttu-id="91663-163">在觀察到背壓之前，伺服器會緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="91663-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="91663-164">增加此值可讓伺服器更快速地緩衝較大的訊息，而不需等待背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="91663-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="91663-165">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="91663-165">All Transports are enabled.</span></span> | <span data-ttu-id="91663-166">`HttpTransportType` 值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="91663-167">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="91663-167">See below.</span></span> | <span data-ttu-id="91663-168">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="91663-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="91663-169">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="91663-169">See below.</span></span> | <span data-ttu-id="91663-170">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="91663-170">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="91663-171">長輪詢傳輸具有其他選項，可以使用 `LongPolling` 屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-171">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="91663-172">選項</span><span class="sxs-lookup"><span data-stu-id="91663-172">Option</span></span> | <span data-ttu-id="91663-173">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-173">Default Value</span></span> | <span data-ttu-id="91663-174">描述</span><span class="sxs-lookup"><span data-stu-id="91663-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="91663-175">90秒</span><span class="sxs-lookup"><span data-stu-id="91663-175">90 seconds</span></span> | <span data-ttu-id="91663-176">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="91663-176">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="91663-177">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="91663-177">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="91663-178">WebSocket 傳輸具有其他選項，可以使用 `WebSockets` 屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-178">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="91663-179">選項</span><span class="sxs-lookup"><span data-stu-id="91663-179">Option</span></span> | <span data-ttu-id="91663-180">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-180">Default Value</span></span> | <span data-ttu-id="91663-181">描述</span><span class="sxs-lookup"><span data-stu-id="91663-181">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="91663-182">5 秒</span><span class="sxs-lookup"><span data-stu-id="91663-182">5 seconds</span></span> | <span data-ttu-id="91663-183">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="91663-183">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="91663-184">可以用來將 `Sec-WebSocket-Protocol` 標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="91663-184">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="91663-185">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="91663-185">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="91663-186">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="91663-186">Configure client options</span></span>

<span data-ttu-id="91663-187">您可以在 `HubConnectionBuilder` 類型上設定用戶端選項（可在 .NET 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="91663-187">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="91663-188">它也可在 JAVA 用戶端中使用，但 `HttpHubConnectionBuilder` 子類別則包含 builder 設定選項，以及 `HubConnection` 本身。</span><span class="sxs-lookup"><span data-stu-id="91663-188">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="91663-189">設定記錄</span><span class="sxs-lookup"><span data-stu-id="91663-189">Configure logging</span></span>

<span data-ttu-id="91663-190">記錄是在 .NET 用戶端中使用 `ConfigureLogging` 方法來設定。</span><span class="sxs-lookup"><span data-stu-id="91663-190">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="91663-191">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="91663-191">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="91663-192">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="91663-192">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="91663-193">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="91663-193">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="91663-194">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="91663-194">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="91663-195">例如，若要啟用主控台記錄，請安裝 `Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="91663-195">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="91663-196">呼叫 `AddConsole` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="91663-196">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="91663-197">在 JavaScript 用戶端中，有類似的 `configureLogging` 方法存在。</span><span class="sxs-lookup"><span data-stu-id="91663-197">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="91663-198">提供 `LogLevel` 值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="91663-198">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="91663-199">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="91663-199">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="91663-200">您也可以提供代表記錄層級名稱的 `string` 值，而不是 `LogLevel` 值。</span><span class="sxs-lookup"><span data-stu-id="91663-200">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="91663-201">在您無法存取 `LogLevel` 常數的環境中設定 SignalR 記錄時，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="91663-201">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="91663-202">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="91663-202">The following table lists the available log levels.</span></span> <span data-ttu-id="91663-203">您提供給 `configureLogging` 的值會設定要記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="91663-203">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="91663-204">記錄在此層級的訊息，**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="91663-204">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="91663-205">String</span><span class="sxs-lookup"><span data-stu-id="91663-205">String</span></span>                      | <span data-ttu-id="91663-206">LogLevel</span><span class="sxs-lookup"><span data-stu-id="91663-206">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="91663-207">`info`**或**`information`</span><span class="sxs-lookup"><span data-stu-id="91663-207">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="91663-208">`warn`**或**`warning`</span><span class="sxs-lookup"><span data-stu-id="91663-208">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="91663-209">若要完全停用記錄，請在 `configureLogging` 方法中指定 `signalR.LogLevel.None`。</span><span class="sxs-lookup"><span data-stu-id="91663-209">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="91663-210">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="91663-210">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="91663-211">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="91663-211">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="91663-212">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="91663-212">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="91663-213">下列程式碼片段示範如何使用 `java.util.logging` 搭配 SignalR JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-213">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="91663-214">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="91663-214">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="91663-215">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="91663-215">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="91663-216">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="91663-216">Configure allowed transports</span></span>

<span data-ttu-id="91663-217">SignalR 所使用的傳輸可以在 `WithUrl` 呼叫中設定（在 JavaScript 中為`withUrl`）。</span><span class="sxs-lookup"><span data-stu-id="91663-217">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="91663-218">`HttpTransportType` 值的位 OR 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-218">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="91663-219">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-219">All transports are enabled by default.</span></span>

<span data-ttu-id="91663-220">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="91663-220">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="91663-221">在 JavaScript 用戶端中，藉由在提供給 `withUrl`的選項物件上設定 [`transport`] 欄位來設定傳輸：</span><span class="sxs-lookup"><span data-stu-id="91663-221">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="91663-222">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-222">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="91663-223">在 JAVA 用戶端中，會使用 `HttpHubConnectionBuilder`上的 `withTransport` 方法來選取傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-223">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="91663-224">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-224">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="91663-225">SignalR JAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="91663-225">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="91663-226">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="91663-226">Configure bearer authentication</span></span>

<span data-ttu-id="91663-227">若要提供驗證資料以及 SignalR 要求，請使用 `AccessTokenProvider` 選項（JavaScript 中的`accessTokenFactory`）來指定會傳回所需存取權杖的函式。</span><span class="sxs-lookup"><span data-stu-id="91663-227">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="91663-228">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用具有 `Bearer`類型的 `Authorization` 標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="91663-228">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="91663-229">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="91663-229">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="91663-230">在這些情況下，存取權杖會當做查詢字串值提供 `access_token`。</span><span class="sxs-lookup"><span data-stu-id="91663-230">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="91663-231">在 .NET 用戶端中，可以使用 `WithUrl`中的選項委派來指定 `AccessTokenProvider` 選項：</span><span class="sxs-lookup"><span data-stu-id="91663-231">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="91663-232">在 JavaScript 用戶端中，存取權杖是藉由在 `withUrl`中的 options 物件上設定 [`accessTokenFactory`] 欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-232">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="91663-233">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-233">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="91663-234">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串 >](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="91663-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="91663-235">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-235">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="91663-236">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="91663-236">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="91663-237">設定 timeout 和 keep-alive 行為的其他選項可用於 `HubConnection` 物件本身：</span><span class="sxs-lookup"><span data-stu-id="91663-237">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="91663-238">.NET</span><span class="sxs-lookup"><span data-stu-id="91663-238">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="91663-239">選項</span><span class="sxs-lookup"><span data-stu-id="91663-239">Option</span></span> | <span data-ttu-id="91663-240">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-240">Default value</span></span> | <span data-ttu-id="91663-241">描述</span><span class="sxs-lookup"><span data-stu-id="91663-241">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="91663-242">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-242">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="91663-243">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-243">Timeout for server activity.</span></span> <span data-ttu-id="91663-244">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（在 JavaScript 中`onclose`）。</span><span class="sxs-lookup"><span data-stu-id="91663-244">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="91663-245">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-245">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="91663-246">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="91663-246">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="91663-247">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-247">15 seconds</span></span> | <span data-ttu-id="91663-248">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-248">Timeout for initial server handshake.</span></span> <span data-ttu-id="91663-249">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（在 JavaScript 中`onclose`）。</span><span class="sxs-lookup"><span data-stu-id="91663-249">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="91663-250">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="91663-250">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="91663-251">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="91663-251">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="91663-252">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-252">15 seconds</span></span> | <span data-ttu-id="91663-253">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="91663-253">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="91663-254">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="91663-254">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="91663-255">如果用戶端未在伺服器上設定的 `ClientTimeoutInterval` 中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-255">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="91663-256">在 .NET 用戶端中，timeout 值會指定為 `TimeSpan` 值。</span><span class="sxs-lookup"><span data-stu-id="91663-256">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="91663-257">JavaScript</span><span class="sxs-lookup"><span data-stu-id="91663-257">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="91663-258">選項</span><span class="sxs-lookup"><span data-stu-id="91663-258">Option</span></span> | <span data-ttu-id="91663-259">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-259">Default value</span></span> | <span data-ttu-id="91663-260">描述</span><span class="sxs-lookup"><span data-stu-id="91663-260">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="91663-261">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-261">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="91663-262">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-262">Timeout for server activity.</span></span> <span data-ttu-id="91663-263">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="91663-263">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="91663-264">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-264">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="91663-265">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="91663-265">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="91663-266">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-266">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="91663-267">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="91663-267">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="91663-268">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="91663-268">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="91663-269">如果用戶端未在伺服器上設定的 `ClientTimeoutInterval` 中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-269">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="91663-270">Java</span><span class="sxs-lookup"><span data-stu-id="91663-270">Java</span></span>](#tab/java)

| <span data-ttu-id="91663-271">選項</span><span class="sxs-lookup"><span data-stu-id="91663-271">Option</span></span> | <span data-ttu-id="91663-272">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-272">Default value</span></span> | <span data-ttu-id="91663-273">描述</span><span class="sxs-lookup"><span data-stu-id="91663-273">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="91663-274">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="91663-274">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="91663-275">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-275">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="91663-276">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-276">Timeout for server activity.</span></span> <span data-ttu-id="91663-277">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="91663-277">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="91663-278">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-278">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="91663-279">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="91663-279">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="91663-280">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-280">15 seconds</span></span> | <span data-ttu-id="91663-281">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-281">Timeout for initial server handshake.</span></span> <span data-ttu-id="91663-282">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="91663-282">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="91663-283">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="91663-283">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="91663-284">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="91663-284">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="91663-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="91663-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="91663-286">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-286">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="91663-287">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="91663-287">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="91663-288">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="91663-288">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="91663-289">如果用戶端未在伺服器上設定的 `ClientTimeoutInterval` 中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-289">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="91663-290">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="91663-290">Configure additional options</span></span>

<span data-ttu-id="91663-291">您可以在 `HubConnectionBuilder` 上或在 JAVA 用戶端的 `HttpHubConnectionBuilder` 上，于 `WithUrl` （在 JavaScript 中`withUrl`）方法中設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="91663-291">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="91663-292">.NET</span><span class="sxs-lookup"><span data-stu-id="91663-292">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="91663-293">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="91663-293">.NET Option</span></span> |  <span data-ttu-id="91663-294">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-294">Default value</span></span> | <span data-ttu-id="91663-295">描述</span><span class="sxs-lookup"><span data-stu-id="91663-295">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="91663-296">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-296">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="91663-297">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="91663-297">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="91663-298">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="91663-298">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="91663-299">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="91663-299">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="91663-300">空白</span><span class="sxs-lookup"><span data-stu-id="91663-300">Empty</span></span> | <span data-ttu-id="91663-301">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="91663-301">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="91663-302">空白</span><span class="sxs-lookup"><span data-stu-id="91663-302">Empty</span></span> | <span data-ttu-id="91663-303">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="91663-303">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="91663-304">空白</span><span class="sxs-lookup"><span data-stu-id="91663-304">Empty</span></span> | <span data-ttu-id="91663-305">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="91663-305">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="91663-306">5 秒</span><span class="sxs-lookup"><span data-stu-id="91663-306">5 seconds</span></span> | <span data-ttu-id="91663-307">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="91663-307">WebSockets only.</span></span> <span data-ttu-id="91663-308">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="91663-308">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="91663-309">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-309">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="91663-310">空白</span><span class="sxs-lookup"><span data-stu-id="91663-310">Empty</span></span> | <span data-ttu-id="91663-311">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="91663-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="91663-312">委派，可以用來設定或取代用來傳送 HTTP 要求的 `HttpMessageHandler`。</span><span class="sxs-lookup"><span data-stu-id="91663-312">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="91663-313">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="91663-313">Not used for WebSocket connections.</span></span> <span data-ttu-id="91663-314">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="91663-314">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="91663-315">請修改該預設值的設定並將其傳回，或傳回新的 `HttpMessageHandler` 實例。</span><span class="sxs-lookup"><span data-stu-id="91663-315">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="91663-316">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="91663-316">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="91663-317">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="91663-317">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="91663-318">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="91663-318">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="91663-319">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="91663-319">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="91663-320">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="91663-320">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="91663-321">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="91663-321">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="91663-322">JavaScript</span><span class="sxs-lookup"><span data-stu-id="91663-322">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="91663-323">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="91663-323">JavaScript Option</span></span> | <span data-ttu-id="91663-324">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-324">Default Value</span></span> | <span data-ttu-id="91663-325">描述</span><span class="sxs-lookup"><span data-stu-id="91663-325">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="91663-326">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-326">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="91663-327">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="91663-327">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="91663-328">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="91663-328">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="91663-329">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="91663-329">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="91663-330">Java</span><span class="sxs-lookup"><span data-stu-id="91663-330">Java</span></span>](#tab/java)

| <span data-ttu-id="91663-331">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="91663-331">Java Option</span></span> | <span data-ttu-id="91663-332">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-332">Default Value</span></span> | <span data-ttu-id="91663-333">描述</span><span class="sxs-lookup"><span data-stu-id="91663-333">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="91663-334">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-334">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="91663-335">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="91663-335">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="91663-336">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="91663-336">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="91663-337">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="91663-337">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="91663-338">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="91663-338">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="91663-339">空白</span><span class="sxs-lookup"><span data-stu-id="91663-339">Empty</span></span> | <span data-ttu-id="91663-340">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="91663-340">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="91663-341">在 .NET 用戶端中，這些選項可以由提供給 `WithUrl`的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="91663-341">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="91663-342">在 JavaScript 用戶端中，可以在提供給 `withUrl`的 JavaScript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="91663-342">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="91663-343">在 JAVA 用戶端中，可以使用從 `HubConnectionBuilder.create("HUB URL")` 傳回的 `HttpHubConnectionBuilder` 上的方法來設定這些選項。</span><span class="sxs-lookup"><span data-stu-id="91663-343">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="91663-344">其他資源</span><span class="sxs-lookup"><span data-stu-id="91663-344">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="91663-345">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="91663-345">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="91663-346">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="91663-346">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="91663-347">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="91663-347">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="91663-348">您可以在伺服器上使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法來設定 JSON 序列化，這可在您的 `Startup.ConfigureServices` 方法中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="91663-348">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="91663-349">`AddJsonProtocol` 方法會接受一個可接收 `options` 物件的委派。</span><span class="sxs-lookup"><span data-stu-id="91663-349">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="91663-350">該物件上的[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性是 JSON.NET `JsonSerializerSettings` 物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="91663-350">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="91663-351">如需詳細資訊，請參閱[JSON.NET 檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="91663-351">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="91663-352">例如，若要設定序列化程式使用 "PascalCase" 屬性名稱，而不是預設的 "camelCase" 名稱，請在 `Startup.ConfigureServices`中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="91663-352">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="91663-353">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="91663-353">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="91663-354">必須匯入 `Microsoft.Extensions.DependencyInjection` 命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="91663-354">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="91663-355">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="91663-355">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="91663-356">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="91663-356">MessagePack serialization options</span></span>

<span data-ttu-id="91663-357">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="91663-357">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="91663-358">如需詳細資訊，請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="91663-358">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="91663-359">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="91663-359">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="91663-360">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="91663-360">Configure server options</span></span>

<span data-ttu-id="91663-361">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="91663-361">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="91663-362">選項</span><span class="sxs-lookup"><span data-stu-id="91663-362">Option</span></span> | <span data-ttu-id="91663-363">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-363">Default Value</span></span> | <span data-ttu-id="91663-364">描述</span><span class="sxs-lookup"><span data-stu-id="91663-364">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="91663-365">30 秒</span><span class="sxs-lookup"><span data-stu-id="91663-365">30 seconds</span></span> | <span data-ttu-id="91663-366">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-366">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="91663-367">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="91663-367">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="91663-368">建議的值是 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="91663-368">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="91663-369">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-369">15 seconds</span></span> | <span data-ttu-id="91663-370">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="91663-370">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="91663-371">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="91663-371">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="91663-372">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="91663-372">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="91663-373">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-373">15 seconds</span></span> | <span data-ttu-id="91663-374">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="91663-374">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="91663-375">當變更 `KeepAliveInterval`時，請變更用戶端上的 `ServerTimeout`/`serverTimeoutInMilliseconds` 設定。</span><span class="sxs-lookup"><span data-stu-id="91663-375">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="91663-376">建議的 `ServerTimeout`/`serverTimeoutInMilliseconds` 值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="91663-376">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="91663-377">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="91663-377">All installed protocols</span></span> | <span data-ttu-id="91663-378">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="91663-378">Protocols supported by this hub.</span></span> <span data-ttu-id="91663-379">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="91663-379">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="91663-380">如果 `true`，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-380">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="91663-381">預設值為 `false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="91663-381">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="91663-382">您可以為所有中樞設定選項，方法是在 `Startup.ConfigureServices`中提供 `AddSignalR` 呼叫的選項委派。</span><span class="sxs-lookup"><span data-stu-id="91663-382">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="91663-383">單一中樞的選項會覆寫 `AddSignalR` 中提供的全域選項，而且可以使用 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-383">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="91663-384">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="91663-384">Advanced HTTP configuration options</span></span>

<span data-ttu-id="91663-385">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="91663-385">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="91663-386">這些選項是藉由將委派傳遞至 `Startup.Configure`中的[MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)來設定。</span><span class="sxs-lookup"><span data-stu-id="91663-386">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="91663-387">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="91663-387">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="91663-388">選項</span><span class="sxs-lookup"><span data-stu-id="91663-388">Option</span></span> | <span data-ttu-id="91663-389">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-389">Default Value</span></span> | <span data-ttu-id="91663-390">描述</span><span class="sxs-lookup"><span data-stu-id="91663-390">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="91663-391">32 KB</span><span class="sxs-lookup"><span data-stu-id="91663-391">32 KB</span></span> | <span data-ttu-id="91663-392">從用戶端接收伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="91663-392">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="91663-393">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="91663-393">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="91663-394">自動從套用至中樞類別的 `Authorize` 屬性收集資料。</span><span class="sxs-lookup"><span data-stu-id="91663-394">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="91663-395">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="91663-395">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="91663-396">32 KB</span><span class="sxs-lookup"><span data-stu-id="91663-396">32 KB</span></span> | <span data-ttu-id="91663-397">由伺服器所緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="91663-397">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="91663-398">增加這個值可讓伺服器傳送較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="91663-398">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="91663-399">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="91663-399">All Transports are enabled.</span></span> | <span data-ttu-id="91663-400">`HttpTransportType` 值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-400">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="91663-401">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="91663-401">See below.</span></span> | <span data-ttu-id="91663-402">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="91663-402">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="91663-403">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="91663-403">See below.</span></span> | <span data-ttu-id="91663-404">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="91663-404">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="91663-405">長輪詢傳輸具有其他選項，可以使用 `LongPolling` 屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-405">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="91663-406">選項</span><span class="sxs-lookup"><span data-stu-id="91663-406">Option</span></span> | <span data-ttu-id="91663-407">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-407">Default Value</span></span> | <span data-ttu-id="91663-408">描述</span><span class="sxs-lookup"><span data-stu-id="91663-408">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="91663-409">90秒</span><span class="sxs-lookup"><span data-stu-id="91663-409">90 seconds</span></span> | <span data-ttu-id="91663-410">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="91663-410">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="91663-411">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="91663-411">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="91663-412">WebSocket 傳輸具有其他選項，可以使用 `WebSockets` 屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-412">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="91663-413">選項</span><span class="sxs-lookup"><span data-stu-id="91663-413">Option</span></span> | <span data-ttu-id="91663-414">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-414">Default Value</span></span> | <span data-ttu-id="91663-415">描述</span><span class="sxs-lookup"><span data-stu-id="91663-415">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="91663-416">5 秒</span><span class="sxs-lookup"><span data-stu-id="91663-416">5 seconds</span></span> | <span data-ttu-id="91663-417">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="91663-417">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="91663-418">可以用來將 `Sec-WebSocket-Protocol` 標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="91663-418">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="91663-419">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="91663-419">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="91663-420">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="91663-420">Configure client options</span></span>

<span data-ttu-id="91663-421">您可以在 `HubConnectionBuilder` 類型上設定用戶端選項（可在 .NET 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="91663-421">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="91663-422">它也可在 JAVA 用戶端中使用，但 `HttpHubConnectionBuilder` 子類別則包含 builder 設定選項，以及 `HubConnection` 本身。</span><span class="sxs-lookup"><span data-stu-id="91663-422">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="91663-423">設定記錄</span><span class="sxs-lookup"><span data-stu-id="91663-423">Configure logging</span></span>

<span data-ttu-id="91663-424">記錄是在 .NET 用戶端中使用 `ConfigureLogging` 方法來設定。</span><span class="sxs-lookup"><span data-stu-id="91663-424">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="91663-425">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="91663-425">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="91663-426">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="91663-426">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="91663-427">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="91663-427">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="91663-428">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="91663-428">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="91663-429">例如，若要啟用主控台記錄，請安裝 `Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="91663-429">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="91663-430">呼叫 `AddConsole` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="91663-430">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="91663-431">在 JavaScript 用戶端中，有類似的 `configureLogging` 方法存在。</span><span class="sxs-lookup"><span data-stu-id="91663-431">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="91663-432">提供 `LogLevel` 值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="91663-432">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="91663-433">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="91663-433">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="91663-434">若要完全停用記錄，請在 `configureLogging` 方法中指定 `signalR.LogLevel.None`。</span><span class="sxs-lookup"><span data-stu-id="91663-434">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="91663-435">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="91663-435">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="91663-436">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="91663-436">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="91663-437">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="91663-437">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="91663-438">下列程式碼片段示範如何使用 `java.util.logging` 搭配 SignalR JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-438">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="91663-439">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="91663-439">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="91663-440">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="91663-440">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="91663-441">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="91663-441">Configure allowed transports</span></span>

<span data-ttu-id="91663-442">SignalR 所使用的傳輸可以在 `WithUrl` 呼叫中設定（在 JavaScript 中為`withUrl`）。</span><span class="sxs-lookup"><span data-stu-id="91663-442">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="91663-443">`HttpTransportType` 值的位 OR 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-443">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="91663-444">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-444">All transports are enabled by default.</span></span>

<span data-ttu-id="91663-445">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="91663-445">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="91663-446">在 JavaScript 用戶端中，藉由在提供給 `withUrl`的選項物件上設定 [`transport`] 欄位來設定傳輸：</span><span class="sxs-lookup"><span data-stu-id="91663-446">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="91663-447">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-447">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="91663-448">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="91663-448">Configure bearer authentication</span></span>

<span data-ttu-id="91663-449">若要提供驗證資料以及 SignalR 要求，請使用 `AccessTokenProvider` 選項（JavaScript 中的`accessTokenFactory`）來指定會傳回所需存取權杖的函式。</span><span class="sxs-lookup"><span data-stu-id="91663-449">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="91663-450">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用具有 `Bearer`類型的 `Authorization` 標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="91663-450">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="91663-451">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="91663-451">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="91663-452">在這些情況下，存取權杖會當做查詢字串值提供 `access_token`。</span><span class="sxs-lookup"><span data-stu-id="91663-452">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="91663-453">在 .NET 用戶端中，可以使用 `WithUrl`中的選項委派來指定 `AccessTokenProvider` 選項：</span><span class="sxs-lookup"><span data-stu-id="91663-453">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="91663-454">在 JavaScript 用戶端中，存取權杖是藉由在 `withUrl`中的 options 物件上設定 [`accessTokenFactory`] 欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-454">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="91663-455">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-455">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="91663-456">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串 >](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="91663-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="91663-457">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-457">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="91663-458">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="91663-458">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="91663-459">設定 timeout 和 keep-alive 行為的其他選項可用於 `HubConnection` 物件本身：</span><span class="sxs-lookup"><span data-stu-id="91663-459">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="91663-460">.NET</span><span class="sxs-lookup"><span data-stu-id="91663-460">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="91663-461">選項</span><span class="sxs-lookup"><span data-stu-id="91663-461">Option</span></span> | <span data-ttu-id="91663-462">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-462">Default value</span></span> | <span data-ttu-id="91663-463">描述</span><span class="sxs-lookup"><span data-stu-id="91663-463">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="91663-464">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-464">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="91663-465">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-465">Timeout for server activity.</span></span> <span data-ttu-id="91663-466">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（在 JavaScript 中`onclose`）。</span><span class="sxs-lookup"><span data-stu-id="91663-466">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="91663-467">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-467">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="91663-468">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="91663-468">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="91663-469">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-469">15 seconds</span></span> | <span data-ttu-id="91663-470">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-470">Timeout for initial server handshake.</span></span> <span data-ttu-id="91663-471">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（在 JavaScript 中`onclose`）。</span><span class="sxs-lookup"><span data-stu-id="91663-471">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="91663-472">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="91663-472">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="91663-473">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="91663-473">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="91663-474">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-474">15 seconds</span></span> | <span data-ttu-id="91663-475">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="91663-475">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="91663-476">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="91663-476">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="91663-477">如果用戶端未在伺服器上設定的 `ClientTimeoutInterval` 中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-477">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="91663-478">在 .NET 用戶端中，timeout 值會指定為 `TimeSpan` 值。</span><span class="sxs-lookup"><span data-stu-id="91663-478">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="91663-479">JavaScript</span><span class="sxs-lookup"><span data-stu-id="91663-479">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="91663-480">選項</span><span class="sxs-lookup"><span data-stu-id="91663-480">Option</span></span> | <span data-ttu-id="91663-481">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-481">Default value</span></span> | <span data-ttu-id="91663-482">描述</span><span class="sxs-lookup"><span data-stu-id="91663-482">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="91663-483">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-483">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="91663-484">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-484">Timeout for server activity.</span></span> <span data-ttu-id="91663-485">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="91663-485">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="91663-486">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-486">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="91663-487">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="91663-487">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="91663-488">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-488">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="91663-489">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="91663-489">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="91663-490">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="91663-490">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="91663-491">如果用戶端未在伺服器上設定的 `ClientTimeoutInterval` 中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-491">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="91663-492">Java</span><span class="sxs-lookup"><span data-stu-id="91663-492">Java</span></span>](#tab/java)

| <span data-ttu-id="91663-493">選項</span><span class="sxs-lookup"><span data-stu-id="91663-493">Option</span></span> | <span data-ttu-id="91663-494">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-494">Default value</span></span> | <span data-ttu-id="91663-495">描述</span><span class="sxs-lookup"><span data-stu-id="91663-495">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="91663-496">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="91663-496">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="91663-497">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-497">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="91663-498">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-498">Timeout for server activity.</span></span> <span data-ttu-id="91663-499">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="91663-499">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="91663-500">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-500">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="91663-501">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="91663-501">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="91663-502">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-502">15 seconds</span></span> | <span data-ttu-id="91663-503">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-503">Timeout for initial server handshake.</span></span> <span data-ttu-id="91663-504">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="91663-504">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="91663-505">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="91663-505">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="91663-506">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="91663-506">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="91663-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="91663-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="91663-508">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-508">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="91663-509">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="91663-509">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="91663-510">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="91663-510">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="91663-511">如果用戶端未在伺服器上設定的 `ClientTimeoutInterval` 中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-511">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="91663-512">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="91663-512">Configure additional options</span></span>

<span data-ttu-id="91663-513">您可以在 `HubConnectionBuilder` 上或在 JAVA 用戶端的 `HttpHubConnectionBuilder` 上，于 `WithUrl` （在 JavaScript 中`withUrl`）方法中設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="91663-513">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="91663-514">.NET</span><span class="sxs-lookup"><span data-stu-id="91663-514">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="91663-515">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="91663-515">.NET Option</span></span> |  <span data-ttu-id="91663-516">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-516">Default value</span></span> | <span data-ttu-id="91663-517">描述</span><span class="sxs-lookup"><span data-stu-id="91663-517">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="91663-518">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-518">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="91663-519">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="91663-519">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="91663-520">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="91663-520">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="91663-521">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="91663-521">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="91663-522">空白</span><span class="sxs-lookup"><span data-stu-id="91663-522">Empty</span></span> | <span data-ttu-id="91663-523">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="91663-523">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="91663-524">空白</span><span class="sxs-lookup"><span data-stu-id="91663-524">Empty</span></span> | <span data-ttu-id="91663-525">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="91663-525">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="91663-526">空白</span><span class="sxs-lookup"><span data-stu-id="91663-526">Empty</span></span> | <span data-ttu-id="91663-527">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="91663-527">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="91663-528">5 秒</span><span class="sxs-lookup"><span data-stu-id="91663-528">5 seconds</span></span> | <span data-ttu-id="91663-529">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="91663-529">WebSockets only.</span></span> <span data-ttu-id="91663-530">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="91663-530">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="91663-531">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-531">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="91663-532">空白</span><span class="sxs-lookup"><span data-stu-id="91663-532">Empty</span></span> | <span data-ttu-id="91663-533">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="91663-533">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="91663-534">委派，可以用來設定或取代用來傳送 HTTP 要求的 `HttpMessageHandler`。</span><span class="sxs-lookup"><span data-stu-id="91663-534">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="91663-535">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="91663-535">Not used for WebSocket connections.</span></span> <span data-ttu-id="91663-536">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="91663-536">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="91663-537">請修改該預設值的設定並將其傳回，或傳回新的 `HttpMessageHandler` 實例。</span><span class="sxs-lookup"><span data-stu-id="91663-537">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="91663-538">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="91663-538">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="91663-539">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="91663-539">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="91663-540">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="91663-540">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="91663-541">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="91663-541">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="91663-542">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="91663-542">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="91663-543">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="91663-543">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="91663-544">JavaScript</span><span class="sxs-lookup"><span data-stu-id="91663-544">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="91663-545">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="91663-545">JavaScript Option</span></span> | <span data-ttu-id="91663-546">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-546">Default Value</span></span> | <span data-ttu-id="91663-547">描述</span><span class="sxs-lookup"><span data-stu-id="91663-547">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="91663-548">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-548">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="91663-549">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="91663-549">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="91663-550">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="91663-550">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="91663-551">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="91663-551">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="91663-552">Java</span><span class="sxs-lookup"><span data-stu-id="91663-552">Java</span></span>](#tab/java)

| <span data-ttu-id="91663-553">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="91663-553">Java Option</span></span> | <span data-ttu-id="91663-554">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-554">Default Value</span></span> | <span data-ttu-id="91663-555">描述</span><span class="sxs-lookup"><span data-stu-id="91663-555">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="91663-556">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-556">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="91663-557">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="91663-557">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="91663-558">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="91663-558">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="91663-559">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="91663-559">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="91663-560">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="91663-560">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="91663-561">空白</span><span class="sxs-lookup"><span data-stu-id="91663-561">Empty</span></span> | <span data-ttu-id="91663-562">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="91663-562">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="91663-563">在 .NET 用戶端中，這些選項可以由提供給 `WithUrl`的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="91663-563">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="91663-564">在 JavaScript 用戶端中，可以在提供給 `withUrl`的 JavaScript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="91663-564">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="91663-565">在 JAVA 用戶端中，可以使用從 `HubConnectionBuilder.create("HUB URL")` 傳回的 `HttpHubConnectionBuilder` 上的方法來設定這些選項。</span><span class="sxs-lookup"><span data-stu-id="91663-565">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="91663-566">其他資源</span><span class="sxs-lookup"><span data-stu-id="91663-566">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="91663-567">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="91663-567">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="91663-568">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="91663-568">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="91663-569">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="91663-569">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="91663-570">您可以在伺服器上使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法來設定 JSON 序列化，這可在您的 `Startup.ConfigureServices` 方法中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="91663-570">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="91663-571">`AddJsonProtocol` 方法會接受一個可接收 `options` 物件的委派。</span><span class="sxs-lookup"><span data-stu-id="91663-571">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="91663-572">該物件上的[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性是 JSON.NET `JsonSerializerSettings` 物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="91663-572">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="91663-573">如需詳細資訊，請參閱[JSON.NET 檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="91663-573">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="91663-574">例如，若要設定序列化程式使用 "PascalCase" 屬性名稱，而不是預設的 "camelCase" 名稱，請在 `Startup.ConfigureServices`中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="91663-574">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="91663-575">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="91663-575">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="91663-576">必須匯入 `Microsoft.Extensions.DependencyInjection` 命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="91663-576">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="91663-577">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="91663-577">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="91663-578">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="91663-578">MessagePack serialization options</span></span>

<span data-ttu-id="91663-579">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="91663-579">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="91663-580">如需詳細資訊，請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="91663-580">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="91663-581">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="91663-581">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="91663-582">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="91663-582">Configure server options</span></span>

<span data-ttu-id="91663-583">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="91663-583">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="91663-584">選項</span><span class="sxs-lookup"><span data-stu-id="91663-584">Option</span></span> | <span data-ttu-id="91663-585">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-585">Default Value</span></span> | <span data-ttu-id="91663-586">描述</span><span class="sxs-lookup"><span data-stu-id="91663-586">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="91663-587">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-587">15 seconds</span></span> | <span data-ttu-id="91663-588">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="91663-588">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="91663-589">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="91663-589">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="91663-590">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="91663-590">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="91663-591">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-591">15 seconds</span></span> | <span data-ttu-id="91663-592">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="91663-592">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="91663-593">當變更 `KeepAliveInterval`時，請變更用戶端上的 `ServerTimeout`/`serverTimeoutInMilliseconds` 設定。</span><span class="sxs-lookup"><span data-stu-id="91663-593">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="91663-594">建議的 `ServerTimeout`/`serverTimeoutInMilliseconds` 值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="91663-594">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="91663-595">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="91663-595">All installed protocols</span></span> | <span data-ttu-id="91663-596">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="91663-596">Protocols supported by this hub.</span></span> <span data-ttu-id="91663-597">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="91663-597">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="91663-598">如果 `true`，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-598">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="91663-599">預設值為 `false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="91663-599">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="91663-600">您可以為所有中樞設定選項，方法是在 `Startup.ConfigureServices`中提供 `AddSignalR` 呼叫的選項委派。</span><span class="sxs-lookup"><span data-stu-id="91663-600">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="91663-601">單一中樞的選項會覆寫 `AddSignalR` 中提供的全域選項，而且可以使用 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-601">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="91663-602">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="91663-602">Advanced HTTP configuration options</span></span>

<span data-ttu-id="91663-603">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="91663-603">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="91663-604">這些選項是藉由將委派傳遞至 `Startup.Configure`中的[MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)來設定。</span><span class="sxs-lookup"><span data-stu-id="91663-604">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="91663-605">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="91663-605">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="91663-606">選項</span><span class="sxs-lookup"><span data-stu-id="91663-606">Option</span></span> | <span data-ttu-id="91663-607">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-607">Default Value</span></span> | <span data-ttu-id="91663-608">描述</span><span class="sxs-lookup"><span data-stu-id="91663-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="91663-609">32 KB</span><span class="sxs-lookup"><span data-stu-id="91663-609">32 KB</span></span> | <span data-ttu-id="91663-610">從用戶端接收伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="91663-610">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="91663-611">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="91663-611">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="91663-612">自動從套用至中樞類別的 `Authorize` 屬性收集資料。</span><span class="sxs-lookup"><span data-stu-id="91663-612">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="91663-613">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="91663-613">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="91663-614">32 KB</span><span class="sxs-lookup"><span data-stu-id="91663-614">32 KB</span></span> | <span data-ttu-id="91663-615">由伺服器所緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="91663-615">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="91663-616">增加這個值可讓伺服器傳送較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="91663-616">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="91663-617">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="91663-617">All Transports are enabled.</span></span> | <span data-ttu-id="91663-618">`HttpTransportType` 值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-618">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="91663-619">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="91663-619">See below.</span></span> | <span data-ttu-id="91663-620">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="91663-620">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="91663-621">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="91663-621">See below.</span></span> | <span data-ttu-id="91663-622">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="91663-622">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="91663-623">長輪詢傳輸具有其他選項，可以使用 `LongPolling` 屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-623">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="91663-624">選項</span><span class="sxs-lookup"><span data-stu-id="91663-624">Option</span></span> | <span data-ttu-id="91663-625">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-625">Default Value</span></span> | <span data-ttu-id="91663-626">描述</span><span class="sxs-lookup"><span data-stu-id="91663-626">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="91663-627">90秒</span><span class="sxs-lookup"><span data-stu-id="91663-627">90 seconds</span></span> | <span data-ttu-id="91663-628">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="91663-628">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="91663-629">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="91663-629">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="91663-630">WebSocket 傳輸具有其他選項，可以使用 `WebSockets` 屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-630">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="91663-631">選項</span><span class="sxs-lookup"><span data-stu-id="91663-631">Option</span></span> | <span data-ttu-id="91663-632">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-632">Default Value</span></span> | <span data-ttu-id="91663-633">描述</span><span class="sxs-lookup"><span data-stu-id="91663-633">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="91663-634">5 秒</span><span class="sxs-lookup"><span data-stu-id="91663-634">5 seconds</span></span> | <span data-ttu-id="91663-635">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="91663-635">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="91663-636">可以用來將 `Sec-WebSocket-Protocol` 標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="91663-636">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="91663-637">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="91663-637">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="91663-638">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="91663-638">Configure client options</span></span>

<span data-ttu-id="91663-639">您可以在 `HubConnectionBuilder` 類型上設定用戶端選項（可在 .NET 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="91663-639">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="91663-640">它也可在 JAVA 用戶端中使用，但 `HttpHubConnectionBuilder` 子類別則包含 builder 設定選項，以及 `HubConnection` 本身。</span><span class="sxs-lookup"><span data-stu-id="91663-640">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="91663-641">設定記錄</span><span class="sxs-lookup"><span data-stu-id="91663-641">Configure logging</span></span>

<span data-ttu-id="91663-642">記錄是在 .NET 用戶端中使用 `ConfigureLogging` 方法來設定。</span><span class="sxs-lookup"><span data-stu-id="91663-642">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="91663-643">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="91663-643">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="91663-644">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="91663-644">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="91663-645">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="91663-645">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="91663-646">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="91663-646">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="91663-647">例如，若要啟用主控台記錄，請安裝 `Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="91663-647">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="91663-648">呼叫 `AddConsole` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="91663-648">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="91663-649">在 JavaScript 用戶端中，有類似的 `configureLogging` 方法存在。</span><span class="sxs-lookup"><span data-stu-id="91663-649">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="91663-650">提供 `LogLevel` 值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="91663-650">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="91663-651">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="91663-651">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="91663-652">若要完全停用記錄，請在 `configureLogging` 方法中指定 `signalR.LogLevel.None`。</span><span class="sxs-lookup"><span data-stu-id="91663-652">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="91663-653">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="91663-653">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="91663-654">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="91663-654">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="91663-655">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="91663-655">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="91663-656">下列程式碼片段示範如何使用 `java.util.logging` 搭配 SignalR JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-656">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="91663-657">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="91663-657">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="91663-658">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="91663-658">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="91663-659">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="91663-659">Configure allowed transports</span></span>

<span data-ttu-id="91663-660">SignalR 所使用的傳輸可以在 `WithUrl` 呼叫中設定（在 JavaScript 中為`withUrl`）。</span><span class="sxs-lookup"><span data-stu-id="91663-660">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="91663-661">`HttpTransportType` 值的位 OR 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-661">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="91663-662">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="91663-662">All transports are enabled by default.</span></span>

<span data-ttu-id="91663-663">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="91663-663">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="91663-664">在 JavaScript 用戶端中，藉由在提供給 `withUrl`的選項物件上設定 [`transport`] 欄位來設定傳輸：</span><span class="sxs-lookup"><span data-stu-id="91663-664">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="91663-665">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="91663-665">Configure bearer authentication</span></span>

<span data-ttu-id="91663-666">若要提供驗證資料以及 SignalR 要求，請使用 `AccessTokenProvider` 選項（JavaScript 中的`accessTokenFactory`）來指定會傳回所需存取權杖的函式。</span><span class="sxs-lookup"><span data-stu-id="91663-666">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="91663-667">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用具有 `Bearer`類型的 `Authorization` 標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="91663-667">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="91663-668">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="91663-668">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="91663-669">在這些情況下，存取權杖會當做查詢字串值提供 `access_token`。</span><span class="sxs-lookup"><span data-stu-id="91663-669">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="91663-670">在 .NET 用戶端中，可以使用 `WithUrl`中的選項委派來指定 `AccessTokenProvider` 選項：</span><span class="sxs-lookup"><span data-stu-id="91663-670">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="91663-671">在 JavaScript 用戶端中，存取權杖是藉由在 `withUrl`中的 options 物件上設定 [`accessTokenFactory`] 欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="91663-671">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="91663-672">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-672">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="91663-673">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串 >](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="91663-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="91663-674">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-674">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="91663-675">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="91663-675">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="91663-676">設定 timeout 和 keep-alive 行為的其他選項可用於 `HubConnection` 物件本身：</span><span class="sxs-lookup"><span data-stu-id="91663-676">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="91663-677">.NET</span><span class="sxs-lookup"><span data-stu-id="91663-677">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="91663-678">選項</span><span class="sxs-lookup"><span data-stu-id="91663-678">Option</span></span> | <span data-ttu-id="91663-679">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-679">Default value</span></span> | <span data-ttu-id="91663-680">描述</span><span class="sxs-lookup"><span data-stu-id="91663-680">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="91663-681">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-681">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="91663-682">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-682">Timeout for server activity.</span></span> <span data-ttu-id="91663-683">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（在 JavaScript 中`onclose`）。</span><span class="sxs-lookup"><span data-stu-id="91663-683">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="91663-684">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-684">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="91663-685">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="91663-685">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="91663-686">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-686">15 seconds</span></span> | <span data-ttu-id="91663-687">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-687">Timeout for initial server handshake.</span></span> <span data-ttu-id="91663-688">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（在 JavaScript 中`onclose`）。</span><span class="sxs-lookup"><span data-stu-id="91663-688">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="91663-689">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="91663-689">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="91663-690">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="91663-690">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="91663-691">在 .NET 用戶端中，timeout 值會指定為 `TimeSpan` 值。</span><span class="sxs-lookup"><span data-stu-id="91663-691">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="91663-692">JavaScript</span><span class="sxs-lookup"><span data-stu-id="91663-692">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="91663-693">選項</span><span class="sxs-lookup"><span data-stu-id="91663-693">Option</span></span> | <span data-ttu-id="91663-694">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-694">Default value</span></span> | <span data-ttu-id="91663-695">描述</span><span class="sxs-lookup"><span data-stu-id="91663-695">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="91663-696">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-696">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="91663-697">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-697">Timeout for server activity.</span></span> <span data-ttu-id="91663-698">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="91663-698">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="91663-699">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-699">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="91663-700">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="91663-700">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="java"></a>[<span data-ttu-id="91663-701">Java</span><span class="sxs-lookup"><span data-stu-id="91663-701">Java</span></span>](#tab/java)

| <span data-ttu-id="91663-702">選項</span><span class="sxs-lookup"><span data-stu-id="91663-702">Option</span></span> | <span data-ttu-id="91663-703">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-703">Default value</span></span> | <span data-ttu-id="91663-704">描述</span><span class="sxs-lookup"><span data-stu-id="91663-704">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="91663-705">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="91663-705">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="91663-706">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="91663-706">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="91663-707">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-707">Timeout for server activity.</span></span> <span data-ttu-id="91663-708">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="91663-708">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="91663-709">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="91663-709">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="91663-710">建議的值是伺服器的 `KeepAliveInterval` 值至少為兩倍的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="91663-710">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="91663-711">15 秒</span><span class="sxs-lookup"><span data-stu-id="91663-711">15 seconds</span></span> | <span data-ttu-id="91663-712">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="91663-712">Timeout for initial server handshake.</span></span> <span data-ttu-id="91663-713">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="91663-713">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="91663-714">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="91663-714">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="91663-715">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="91663-715">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="91663-716">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="91663-716">Configure additional options</span></span>

<span data-ttu-id="91663-717">您可以在 `HubConnectionBuilder` 上或在 JAVA 用戶端的 `HttpHubConnectionBuilder` 上，于 `WithUrl` （在 JavaScript 中`withUrl`）方法中設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="91663-717">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="91663-718">.NET</span><span class="sxs-lookup"><span data-stu-id="91663-718">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="91663-719">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="91663-719">.NET Option</span></span> |  <span data-ttu-id="91663-720">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-720">Default value</span></span> | <span data-ttu-id="91663-721">描述</span><span class="sxs-lookup"><span data-stu-id="91663-721">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="91663-722">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-722">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="91663-723">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="91663-723">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="91663-724">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="91663-724">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="91663-725">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="91663-725">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="91663-726">空白</span><span class="sxs-lookup"><span data-stu-id="91663-726">Empty</span></span> | <span data-ttu-id="91663-727">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="91663-727">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="91663-728">空白</span><span class="sxs-lookup"><span data-stu-id="91663-728">Empty</span></span> | <span data-ttu-id="91663-729">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="91663-729">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="91663-730">空白</span><span class="sxs-lookup"><span data-stu-id="91663-730">Empty</span></span> | <span data-ttu-id="91663-731">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="91663-731">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="91663-732">5 秒</span><span class="sxs-lookup"><span data-stu-id="91663-732">5 seconds</span></span> | <span data-ttu-id="91663-733">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="91663-733">WebSockets only.</span></span> <span data-ttu-id="91663-734">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="91663-734">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="91663-735">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="91663-735">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="91663-736">空白</span><span class="sxs-lookup"><span data-stu-id="91663-736">Empty</span></span> | <span data-ttu-id="91663-737">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="91663-737">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="91663-738">委派，可以用來設定或取代用來傳送 HTTP 要求的 `HttpMessageHandler`。</span><span class="sxs-lookup"><span data-stu-id="91663-738">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="91663-739">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="91663-739">Not used for WebSocket connections.</span></span> <span data-ttu-id="91663-740">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="91663-740">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="91663-741">請修改該預設值的設定並將其傳回，或傳回新的 `HttpMessageHandler` 實例。</span><span class="sxs-lookup"><span data-stu-id="91663-741">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="91663-742">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="91663-742">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="91663-743">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="91663-743">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="91663-744">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="91663-744">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="91663-745">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="91663-745">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="91663-746">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="91663-746">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="91663-747">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="91663-747">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="91663-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="91663-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="91663-749">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="91663-749">JavaScript Option</span></span> | <span data-ttu-id="91663-750">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-750">Default Value</span></span> | <span data-ttu-id="91663-751">描述</span><span class="sxs-lookup"><span data-stu-id="91663-751">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="91663-752">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-752">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="91663-753">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="91663-753">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="91663-754">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="91663-754">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="91663-755">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="91663-755">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="91663-756">Java</span><span class="sxs-lookup"><span data-stu-id="91663-756">Java</span></span>](#tab/java)

| <span data-ttu-id="91663-757">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="91663-757">Java Option</span></span> | <span data-ttu-id="91663-758">預設值</span><span class="sxs-lookup"><span data-stu-id="91663-758">Default Value</span></span> | <span data-ttu-id="91663-759">描述</span><span class="sxs-lookup"><span data-stu-id="91663-759">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="91663-760">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="91663-760">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="91663-761">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="91663-761">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="91663-762">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="91663-762">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="91663-763">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="91663-763">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="91663-764">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="91663-764">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="91663-765">空白</span><span class="sxs-lookup"><span data-stu-id="91663-765">Empty</span></span> | <span data-ttu-id="91663-766">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="91663-766">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="91663-767">在 .NET 用戶端中，這些選項可以由提供給 `WithUrl`的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="91663-767">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="91663-768">在 JavaScript 用戶端中，可以在提供給 `withUrl`的 JavaScript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="91663-768">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="91663-769">在 JAVA 用戶端中，可以使用從 `HubConnectionBuilder.create("HUB URL")` 傳回的 `HttpHubConnectionBuilder` 上的方法來設定這些選項。</span><span class="sxs-lookup"><span data-stu-id="91663-769">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="91663-770">其他資源</span><span class="sxs-lookup"><span data-stu-id="91663-770">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end