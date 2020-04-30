---
title: ASP.NET Core SignalR設定
author: bradygaster
description: 瞭解如何設定 ASP.NET Core SignalR應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/12/2020
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: 7e0cd952fd152ff6adb6e0a7c56214d70d3c7b86
ms.sourcegitcommit: f9a5069577e8f7c53f8bcec9e13e117950f4f033
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "82559005"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="543f3-103">ASP.NET Core SignalR 組態</span><span class="sxs-lookup"><span data-stu-id="543f3-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-5.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="543f3-104">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="543f3-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="543f3-105">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="543f3-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="543f3-106">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="543f3-107">您可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，在伺服器上設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="543f3-108">`AddJsonProtocol`可以在中`Startup.ConfigureServices`的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="543f3-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="543f3-109">`AddJsonProtocol`方法會接受可接收`options`物件的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="543f3-110">該物件上的[PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是`System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions>物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="543f3-111">如需詳細資訊，請參閱[system.web 檔](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="543f3-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="543f3-112">例如，若要設定序列化程式不要變更屬性名稱的大小寫，而不是預設的 "camelCase" 名稱，請在中`Startup.ConfigureServices`使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="543f3-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="543f3-113">在 .NET 用戶端中，相同`AddJsonProtocol`的擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="543f3-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="543f3-114">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543f3-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="543f3-115">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="543f3-116">切換至 Newtonsoft. Json</span><span class="sxs-lookup"><span data-stu-id="543f3-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="543f3-117">如果您需要中`Newtonsoft.Json` `System.Text.Json`不支援的功能，請參閱[切換至 Newtonsoft。](xref:migration/22-to-30#switch-to-newtonsoftjson)</span><span class="sxs-lookup"><span data-stu-id="543f3-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="543f3-118">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="543f3-118">MessagePack serialization options</span></span>

<span data-ttu-id="543f3-119">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="543f3-120">如需詳細資訊，請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="543f3-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="543f3-121">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="543f3-122">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="543f3-122">Configure server options</span></span>

<span data-ttu-id="543f3-123">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="543f3-124">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-124">Option</span></span> | <span data-ttu-id="543f3-125">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-125">Default Value</span></span> | <span data-ttu-id="543f3-126">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="543f3-127">30 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-127">30 seconds</span></span> | <span data-ttu-id="543f3-128">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="543f3-129">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="543f3-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="543f3-130">建議的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="543f3-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="543f3-131">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-131">15 seconds</span></span> | <span data-ttu-id="543f3-132">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="543f3-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="543f3-133">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-134">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="543f3-135">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-135">15 seconds</span></span> | <span data-ttu-id="543f3-136">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="543f3-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="543f3-137">在變更`KeepAliveInterval`時，請`ServerTimeout` / `serverTimeoutInMilliseconds`變更用戶端上的設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="543f3-138">`ServerTimeout` /建議`serverTimeoutInMilliseconds`的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="543f3-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="543f3-139">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="543f3-139">All installed protocols</span></span> | <span data-ttu-id="543f3-140">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543f3-140">Protocols supported by this hub.</span></span> <span data-ttu-id="543f3-141">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543f3-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="543f3-142">如果`true`為，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="543f3-143">預設值為`false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="543f3-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="543f3-144">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="543f3-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="543f3-145">若達到此限制，則在伺服器處理資料流程專案之前，會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="543f3-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="543f3-146">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-146">32 KB</span></span> | <span data-ttu-id="543f3-147">單一傳入中樞訊息的大小上限。</span><span class="sxs-lookup"><span data-stu-id="543f3-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="543f3-148">您可以為所有中樞設定選項，方法是提供選項委派給`AddSignalR`中`Startup.ConfigureServices`的呼叫。</span><span class="sxs-lookup"><span data-stu-id="543f3-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="543f3-149">單一中樞的選項會覆寫中提供的全域`AddSignalR`選項，並可使用<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>下列方式進行設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="543f3-150">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="543f3-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="543f3-151">使用`HttpConnectionDispatcherOptions`來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="543f3-152">這些選項的設定方式是將委派傳遞至中`Startup.Configure`的[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) 。</span><span class="sxs-lookup"><span data-stu-id="543f3-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="543f3-153">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="543f3-154">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-154">Option</span></span> | <span data-ttu-id="543f3-155">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-155">Default Value</span></span> | <span data-ttu-id="543f3-156">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="543f3-157">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-157">32 KB</span></span> | <span data-ttu-id="543f3-158">在套用背壓之前，伺服器會緩衝的用戶端接收的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="543f3-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="543f3-159">增加此值可讓伺服器更快速地接收較大的訊息，而不需套用背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="543f3-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="543f3-160">從套用至中樞類別`Authorize`的屬性自動收集的資料。</span><span class="sxs-lookup"><span data-stu-id="543f3-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="543f3-161">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="543f3-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="543f3-162">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-162">32 KB</span></span> | <span data-ttu-id="543f3-163">在觀察到背壓之前，伺服器會緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="543f3-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="543f3-164">增加此值可讓伺服器更快速地緩衝較大的訊息，而不需等待背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="543f3-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="543f3-165">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="543f3-165">All Transports are enabled.</span></span> | <span data-ttu-id="543f3-166">`HttpTransportType`值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="543f3-167">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="543f3-167">See below.</span></span> | <span data-ttu-id="543f3-168">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="543f3-169">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="543f3-169">See below.</span></span> | <span data-ttu-id="543f3-170">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-170">Additional options specific to the WebSockets transport.</span></span> |
| `MinimumProtocolVersion` | <span data-ttu-id="543f3-171">0</span><span class="sxs-lookup"><span data-stu-id="543f3-171">0</span></span> | <span data-ttu-id="543f3-172">指定 negotiate 通訊協定的最低版本。</span><span class="sxs-lookup"><span data-stu-id="543f3-172">Specify the minimum version of the negotiate protocol.</span></span> <span data-ttu-id="543f3-173">這是用來將用戶端限制為較新的版本。</span><span class="sxs-lookup"><span data-stu-id="543f3-173">This is used to limit clients to newer versions.</span></span> |

<span data-ttu-id="543f3-174">長輪詢傳輸具有其他選項，可以使用`LongPolling`屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-174">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="543f3-175">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-175">Option</span></span> | <span data-ttu-id="543f3-176">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-176">Default Value</span></span> | <span data-ttu-id="543f3-177">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-177">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="543f3-178">90秒</span><span class="sxs-lookup"><span data-stu-id="543f3-178">90 seconds</span></span> | <span data-ttu-id="543f3-179">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="543f3-179">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="543f3-180">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="543f3-180">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="543f3-181">WebSocket 傳輸具有可使用`WebSockets`屬性來設定的其他選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-181">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="543f3-182">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-182">Option</span></span> | <span data-ttu-id="543f3-183">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-183">Default Value</span></span> | <span data-ttu-id="543f3-184">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-184">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="543f3-185">5 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-185">5 seconds</span></span> | <span data-ttu-id="543f3-186">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="543f3-186">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="543f3-187">可以用來將`Sec-WebSocket-Protocol`標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-187">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="543f3-188">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="543f3-188">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="543f3-189">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="543f3-189">Configure client options</span></span>

<span data-ttu-id="543f3-190">您可以在`HubConnectionBuilder`類型上設定用戶端選項（可在 .Net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="543f3-190">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="543f3-191">它也可在 JAVA 用戶端中使用，但`HttpHubConnectionBuilder`子類別則包含產生器設定選項，以及`HubConnection`本身。</span><span class="sxs-lookup"><span data-stu-id="543f3-191">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="543f3-192">設定記錄</span><span class="sxs-lookup"><span data-stu-id="543f3-192">Configure logging</span></span>

<span data-ttu-id="543f3-193">使用`ConfigureLogging`方法，在 .Net 用戶端中設定記錄。</span><span class="sxs-lookup"><span data-stu-id="543f3-193">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="543f3-194">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="543f3-194">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="543f3-195">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="543f3-195">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="543f3-196">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="543f3-196">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="543f3-197">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="543f3-197">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="543f3-198">例如，若要啟用主控台記錄，請安裝`Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="543f3-198">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="543f3-199">呼叫`AddConsole`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543f3-199">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="543f3-200">在 JavaScript 用戶端中，有`configureLogging`類似的方法存在。</span><span class="sxs-lookup"><span data-stu-id="543f3-200">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="543f3-201">提供`LogLevel`值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-201">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="543f3-202">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="543f3-202">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="543f3-203">您也可以`LogLevel`提供代表記錄層級名稱的`string`值，而不是值。</span><span class="sxs-lookup"><span data-stu-id="543f3-203">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="543f3-204">在您無法存取常數的`LogLevel`環境中設定 SignalR 記錄時，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="543f3-204">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="543f3-205">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-205">The following table lists the available log levels.</span></span> <span data-ttu-id="543f3-206">您所提供的值`configureLogging` ，用來設定將記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-206">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="543f3-207">記錄在此層級的訊息，**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="543f3-207">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="543f3-208">String</span><span class="sxs-lookup"><span data-stu-id="543f3-208">String</span></span>                      | <span data-ttu-id="543f3-209">LogLevel</span><span class="sxs-lookup"><span data-stu-id="543f3-209">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="543f3-210">`info` **或** `information`</span><span class="sxs-lookup"><span data-stu-id="543f3-210">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="543f3-211">`warn` **或** `warning`</span><span class="sxs-lookup"><span data-stu-id="543f3-211">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="543f3-212">若要完全停用記錄`signalR.LogLevel.None` ，請`configureLogging`在方法中指定。</span><span class="sxs-lookup"><span data-stu-id="543f3-212">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="543f3-213">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="543f3-213">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="543f3-214">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="543f3-214">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="543f3-215">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="543f3-215">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="543f3-216">下列程式碼片段示範如何搭配 SignalR JAVA `java.util.logging`用戶端使用。</span><span class="sxs-lookup"><span data-stu-id="543f3-216">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="543f3-217">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="543f3-217">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="543f3-218">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="543f3-218">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="543f3-219">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="543f3-219">Configure allowed transports</span></span>

<span data-ttu-id="543f3-220">SignalR 所使用的傳輸可以在`WithUrl`呼叫中設定（`withUrl`以 JavaScript 為限）。</span><span class="sxs-lookup"><span data-stu-id="543f3-220">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="543f3-221">值的`HttpTransportType`位 or 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-221">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="543f3-222">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-222">All transports are enabled by default.</span></span>

<span data-ttu-id="543f3-223">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="543f3-223">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="543f3-224">在 JavaScript 用戶端中，會透過在提供給`transport` `withUrl`的選項物件上設定欄位，來設定傳輸：</span><span class="sxs-lookup"><span data-stu-id="543f3-224">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="543f3-225">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-225">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="543f3-226">在 JAVA 用戶端中，會使用上的`withTransport`方法來選取傳輸`HttpHubConnectionBuilder`。</span><span class="sxs-lookup"><span data-stu-id="543f3-226">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="543f3-227">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-227">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="543f3-228">SignalR JAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="543f3-228">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="543f3-229">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="543f3-229">Configure bearer authentication</span></span>

<span data-ttu-id="543f3-230">若要提供驗證資料以及 SignalR 要求，請使用`AccessTokenProvider`選項（`accessTokenFactory`在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-230">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="543f3-231">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用類型為`Authorization`的`Bearer`標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="543f3-231">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="543f3-232">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-232">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="543f3-233">在這些情況下，存取權杖會以查詢字串值`access_token`的形式提供。</span><span class="sxs-lookup"><span data-stu-id="543f3-233">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="543f3-234">在 .NET 用戶端中， `AccessTokenProvider`可以使用中`WithUrl`的選項委派來指定選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-234">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="543f3-235">在 JavaScript 用戶端中，存取權杖是藉由在`accessTokenFactory` `withUrl`的選項物件上設定欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-235">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="543f3-236">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-236">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="543f3-237">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串>](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="543f3-237">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="543f3-238">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-238">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="543f3-239">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-239">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="543f3-240">設定 timeout 和 keep-alive 行為的其他選項可用於`HubConnection`物件本身：</span><span class="sxs-lookup"><span data-stu-id="543f3-240">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="543f3-241">.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-241">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="543f3-242">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-242">Option</span></span> | <span data-ttu-id="543f3-243">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-243">Default value</span></span> | <span data-ttu-id="543f3-244">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-244">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="543f3-245">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-245">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-246">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-246">Timeout for server activity.</span></span> <span data-ttu-id="543f3-247">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `Closed`並觸發`onclose`事件（在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-247">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="543f3-248">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-248">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-249">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-249">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="543f3-250">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-250">15 seconds</span></span> | <span data-ttu-id="543f3-251">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-251">Timeout for initial server handshake.</span></span> <span data-ttu-id="543f3-252">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`Closed`事件（`onclose`在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-252">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="543f3-253">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-253">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-254">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-254">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="543f3-255">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-255">15 seconds</span></span> | <span data-ttu-id="543f3-256">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-256">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-257">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-257">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-258">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-258">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="543f3-259">在 .NET 用戶端中，timeout 值會指定`TimeSpan`為值。</span><span class="sxs-lookup"><span data-stu-id="543f3-259">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="543f3-260">JavaScript</span><span class="sxs-lookup"><span data-stu-id="543f3-260">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="543f3-261">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-261">Option</span></span> | <span data-ttu-id="543f3-262">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-262">Default value</span></span> | <span data-ttu-id="543f3-263">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-263">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="543f3-264">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-264">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-265">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-265">Timeout for server activity.</span></span> <span data-ttu-id="543f3-266">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onclose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-266">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="543f3-267">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-267">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-268">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-268">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="543f3-269">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-269">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="543f3-270">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-270">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-271">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-271">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-272">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-272">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="543f3-273">Java</span><span class="sxs-lookup"><span data-stu-id="543f3-273">Java</span></span>](#tab/java)

| <span data-ttu-id="543f3-274">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-274">Option</span></span> | <span data-ttu-id="543f3-275">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-275">Default value</span></span> | <span data-ttu-id="543f3-276">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-276">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="543f3-277">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="543f3-277">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="543f3-278">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-278">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-279">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-279">Timeout for server activity.</span></span> <span data-ttu-id="543f3-280">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onClose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-280">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="543f3-281">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-281">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-282">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-282">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="543f3-283">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-283">15 seconds</span></span> | <span data-ttu-id="543f3-284">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-284">Timeout for initial server handshake.</span></span> <span data-ttu-id="543f3-285">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-285">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="543f3-286">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-286">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-287">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-287">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="543f3-288">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="543f3-288">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="543f3-289">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-289">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="543f3-290">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-290">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-291">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-291">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-292">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-292">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="543f3-293">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="543f3-293">Configure additional options</span></span>

<span data-ttu-id="543f3-294">您`WithUrl`可以在上`withUrl` `HubConnectionBuilder`的（JavaScript）方法中，或在 JAVA 用戶端`HttpHubConnectionBuilder`中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-294">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="543f3-295">.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-295">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="543f3-296">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-296">.NET Option</span></span> |  <span data-ttu-id="543f3-297">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-297">Default value</span></span> | <span data-ttu-id="543f3-298">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-298">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="543f3-299">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-299">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="543f3-300">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-300">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-301">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-301">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-302">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-302">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="543f3-303">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-303">Empty</span></span> | <span data-ttu-id="543f3-304">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="543f3-304">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="543f3-305">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-305">Empty</span></span> | <span data-ttu-id="543f3-306">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="543f3-306">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="543f3-307">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-307">Empty</span></span> | <span data-ttu-id="543f3-308">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-308">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="543f3-309">5 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-309">5 seconds</span></span> | <span data-ttu-id="543f3-310">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="543f3-310">WebSockets only.</span></span> <span data-ttu-id="543f3-311">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="543f3-311">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="543f3-312">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-312">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="543f3-313">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-313">Empty</span></span> | <span data-ttu-id="543f3-314">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="543f3-314">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="543f3-315">委派，可以用來設定或取代用來傳送`HttpMessageHandler` HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="543f3-315">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="543f3-316">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="543f3-316">Not used for WebSocket connections.</span></span> <span data-ttu-id="543f3-317">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="543f3-317">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="543f3-318">請修改該預設值的設定並加以傳回，或傳回新`HttpMessageHandler`的實例。</span><span class="sxs-lookup"><span data-stu-id="543f3-318">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="543f3-319">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="543f3-319">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="543f3-320">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="543f3-320">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="543f3-321">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-321">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="543f3-322">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="543f3-322">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="543f3-323">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-323">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="543f3-324">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="543f3-324">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="543f3-325">JavaScript</span><span class="sxs-lookup"><span data-stu-id="543f3-325">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="543f3-326">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-326">JavaScript Option</span></span> | <span data-ttu-id="543f3-327">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-327">Default Value</span></span> | <span data-ttu-id="543f3-328">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-328">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="543f3-329">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-329">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="543f3-330">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-330">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-331">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-331">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-332">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-332">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `withCredentials` | `true` | <span data-ttu-id="543f3-333">指定是否要使用 CORS 要求來傳送認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-333">Specifies whether credentials will be sent with the CORS request.</span></span> <span data-ttu-id="543f3-334">Azure App Service 使用適用于粘滯會話的 cookie，而且需要啟用此選項才能正常運作。</span><span class="sxs-lookup"><span data-stu-id="543f3-334">Azure App Service uses cookies for sticky sessions and needs this option enabled to work correctly.</span></span> <span data-ttu-id="543f3-335">如需有關 CORS 與 SignalR 的詳細資訊<xref:signalr/security#cross-origin-resource-sharing>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="543f3-335">For more info on CORS with SignalR, see <xref:signalr/security#cross-origin-resource-sharing>.</span></span> |

# <a name="java"></a>[<span data-ttu-id="543f3-336">Java</span><span class="sxs-lookup"><span data-stu-id="543f3-336">Java</span></span>](#tab/java)

| <span data-ttu-id="543f3-337">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-337">Java Option</span></span> | <span data-ttu-id="543f3-338">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-338">Default Value</span></span> | <span data-ttu-id="543f3-339">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-339">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="543f3-340">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-340">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="543f3-341">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-341">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-342">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-342">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-343">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-343">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="543f3-344">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="543f3-344">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="543f3-345">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-345">Empty</span></span> | <span data-ttu-id="543f3-346">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="543f3-346">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="543f3-347">在 .NET 用戶端中，這些選項可以由提供給`WithUrl`的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="543f3-347">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="543f3-348">在 JavaScript 用戶端中，可以在提供給`withUrl`的 javascript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-348">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="543f3-349">在 JAVA 用戶端中，您可以使用所`HttpHubConnectionBuilder`傳回之上的方法來設定這些選項。`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="543f3-349">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="543f3-350">其他資源</span><span class="sxs-lookup"><span data-stu-id="543f3-350">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-3.1"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="543f3-351">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="543f3-351">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="543f3-352">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="543f3-352">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="543f3-353">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-353">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="543f3-354">您可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，在伺服器上設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-354">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="543f3-355">`AddJsonProtocol`可以在中`Startup.ConfigureServices`的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="543f3-355">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="543f3-356">`AddJsonProtocol`方法會接受可接收`options`物件的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-356">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="543f3-357">該物件上的[PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是`System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions>物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-357">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="543f3-358">如需詳細資訊，請參閱[system.web 檔](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="543f3-358">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="543f3-359">例如，若要設定序列化程式不要變更屬性名稱的大小寫，而不是預設的 "camelCase" 名稱，請在中`Startup.ConfigureServices`使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="543f3-359">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="543f3-360">在 .NET 用戶端中，相同`AddJsonProtocol`的擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="543f3-360">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="543f3-361">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543f3-361">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="543f3-362">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-362">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="543f3-363">切換至 Newtonsoft. Json</span><span class="sxs-lookup"><span data-stu-id="543f3-363">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="543f3-364">如果您需要中`Newtonsoft.Json` `System.Text.Json`不支援的功能，請參閱[切換至 Newtonsoft。](xref:migration/22-to-30#switch-to-newtonsoftjson)</span><span class="sxs-lookup"><span data-stu-id="543f3-364">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="543f3-365">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="543f3-365">MessagePack serialization options</span></span>

<span data-ttu-id="543f3-366">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-366">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="543f3-367">如需詳細資訊，請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="543f3-367">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="543f3-368">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-368">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="543f3-369">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="543f3-369">Configure server options</span></span>

<span data-ttu-id="543f3-370">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-370">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="543f3-371">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-371">Option</span></span> | <span data-ttu-id="543f3-372">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-372">Default Value</span></span> | <span data-ttu-id="543f3-373">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-373">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="543f3-374">30 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-374">30 seconds</span></span> | <span data-ttu-id="543f3-375">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-375">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="543f3-376">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="543f3-376">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="543f3-377">建議的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="543f3-377">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="543f3-378">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-378">15 seconds</span></span> | <span data-ttu-id="543f3-379">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="543f3-379">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="543f3-380">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-380">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-381">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-381">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="543f3-382">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-382">15 seconds</span></span> | <span data-ttu-id="543f3-383">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="543f3-383">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="543f3-384">在變更`KeepAliveInterval`時，請`ServerTimeout` / `serverTimeoutInMilliseconds`變更用戶端上的設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-384">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="543f3-385">`ServerTimeout` /建議`serverTimeoutInMilliseconds`的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="543f3-385">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="543f3-386">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="543f3-386">All installed protocols</span></span> | <span data-ttu-id="543f3-387">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543f3-387">Protocols supported by this hub.</span></span> <span data-ttu-id="543f3-388">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543f3-388">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="543f3-389">如果`true`為，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-389">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="543f3-390">預設值為`false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="543f3-390">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="543f3-391">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="543f3-391">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="543f3-392">若達到此限制，則在伺服器處理資料流程專案之前，會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="543f3-392">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="543f3-393">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-393">32 KB</span></span> | <span data-ttu-id="543f3-394">單一傳入中樞訊息的大小上限。</span><span class="sxs-lookup"><span data-stu-id="543f3-394">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="543f3-395">您可以為所有中樞設定選項，方法是提供選項委派給`AddSignalR`中`Startup.ConfigureServices`的呼叫。</span><span class="sxs-lookup"><span data-stu-id="543f3-395">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="543f3-396">單一中樞的選項會覆寫中提供的全域`AddSignalR`選項，並可使用<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>下列方式進行設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-396">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="543f3-397">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="543f3-397">Advanced HTTP configuration options</span></span>

<span data-ttu-id="543f3-398">使用`HttpConnectionDispatcherOptions`來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-398">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="543f3-399">這些選項的設定方式是將委派傳遞至中`Startup.Configure`的[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) 。</span><span class="sxs-lookup"><span data-stu-id="543f3-399">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="543f3-400">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-400">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="543f3-401">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-401">Option</span></span> | <span data-ttu-id="543f3-402">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-402">Default Value</span></span> | <span data-ttu-id="543f3-403">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-403">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="543f3-404">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-404">32 KB</span></span> | <span data-ttu-id="543f3-405">在套用背壓之前，伺服器會緩衝的用戶端接收的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="543f3-405">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="543f3-406">增加此值可讓伺服器更快速地接收較大的訊息，而不需套用背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="543f3-406">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="543f3-407">從套用至中樞類別`Authorize`的屬性自動收集的資料。</span><span class="sxs-lookup"><span data-stu-id="543f3-407">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="543f3-408">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="543f3-408">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="543f3-409">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-409">32 KB</span></span> | <span data-ttu-id="543f3-410">在觀察到背壓之前，伺服器會緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="543f3-410">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="543f3-411">增加此值可讓伺服器更快速地緩衝較大的訊息，而不需等待背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="543f3-411">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="543f3-412">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="543f3-412">All Transports are enabled.</span></span> | <span data-ttu-id="543f3-413">`HttpTransportType`值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-413">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="543f3-414">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="543f3-414">See below.</span></span> | <span data-ttu-id="543f3-415">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-415">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="543f3-416">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="543f3-416">See below.</span></span> | <span data-ttu-id="543f3-417">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-417">Additional options specific to the WebSockets transport.</span></span> |
| `MinimumProtocolVersion` | <span data-ttu-id="543f3-418">0</span><span class="sxs-lookup"><span data-stu-id="543f3-418">0</span></span> | <span data-ttu-id="543f3-419">指定 negotiate 通訊協定的最低版本。</span><span class="sxs-lookup"><span data-stu-id="543f3-419">Specify the minimum version of the negotiate protocol.</span></span> <span data-ttu-id="543f3-420">這是用來將用戶端限制為較新的版本。</span><span class="sxs-lookup"><span data-stu-id="543f3-420">This is used to limit clients to newer versions.</span></span> |

<span data-ttu-id="543f3-421">長輪詢傳輸具有其他選項，可以使用`LongPolling`屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-421">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="543f3-422">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-422">Option</span></span> | <span data-ttu-id="543f3-423">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-423">Default Value</span></span> | <span data-ttu-id="543f3-424">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-424">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="543f3-425">90秒</span><span class="sxs-lookup"><span data-stu-id="543f3-425">90 seconds</span></span> | <span data-ttu-id="543f3-426">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="543f3-426">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="543f3-427">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="543f3-427">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="543f3-428">WebSocket 傳輸具有可使用`WebSockets`屬性來設定的其他選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-428">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="543f3-429">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-429">Option</span></span> | <span data-ttu-id="543f3-430">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-430">Default Value</span></span> | <span data-ttu-id="543f3-431">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-431">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="543f3-432">5 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-432">5 seconds</span></span> | <span data-ttu-id="543f3-433">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="543f3-433">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="543f3-434">可以用來將`Sec-WebSocket-Protocol`標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-434">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="543f3-435">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="543f3-435">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="543f3-436">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="543f3-436">Configure client options</span></span>

<span data-ttu-id="543f3-437">您可以在`HubConnectionBuilder`類型上設定用戶端選項（可在 .Net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="543f3-437">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="543f3-438">它也可在 JAVA 用戶端中使用，但`HttpHubConnectionBuilder`子類別則包含產生器設定選項，以及`HubConnection`本身。</span><span class="sxs-lookup"><span data-stu-id="543f3-438">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="543f3-439">設定記錄</span><span class="sxs-lookup"><span data-stu-id="543f3-439">Configure logging</span></span>

<span data-ttu-id="543f3-440">使用`ConfigureLogging`方法，在 .Net 用戶端中設定記錄。</span><span class="sxs-lookup"><span data-stu-id="543f3-440">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="543f3-441">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="543f3-441">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="543f3-442">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="543f3-442">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="543f3-443">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="543f3-443">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="543f3-444">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="543f3-444">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="543f3-445">例如，若要啟用主控台記錄，請安裝`Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="543f3-445">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="543f3-446">呼叫`AddConsole`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543f3-446">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="543f3-447">在 JavaScript 用戶端中，有`configureLogging`類似的方法存在。</span><span class="sxs-lookup"><span data-stu-id="543f3-447">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="543f3-448">提供`LogLevel`值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-448">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="543f3-449">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="543f3-449">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="543f3-450">您也可以`LogLevel`提供代表記錄層級名稱的`string`值，而不是值。</span><span class="sxs-lookup"><span data-stu-id="543f3-450">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="543f3-451">在您無法存取常數的`LogLevel`環境中設定 SignalR 記錄時，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="543f3-451">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="543f3-452">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-452">The following table lists the available log levels.</span></span> <span data-ttu-id="543f3-453">您所提供的值`configureLogging` ，用來設定將記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-453">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="543f3-454">記錄在此層級的訊息，**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="543f3-454">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="543f3-455">String</span><span class="sxs-lookup"><span data-stu-id="543f3-455">String</span></span>                      | <span data-ttu-id="543f3-456">LogLevel</span><span class="sxs-lookup"><span data-stu-id="543f3-456">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="543f3-457">`info` **或** `information`</span><span class="sxs-lookup"><span data-stu-id="543f3-457">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="543f3-458">`warn` **或** `warning`</span><span class="sxs-lookup"><span data-stu-id="543f3-458">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="543f3-459">若要完全停用記錄`signalR.LogLevel.None` ，請`configureLogging`在方法中指定。</span><span class="sxs-lookup"><span data-stu-id="543f3-459">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="543f3-460">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="543f3-460">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="543f3-461">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="543f3-461">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="543f3-462">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="543f3-462">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="543f3-463">下列程式碼片段示範如何搭配 SignalR JAVA `java.util.logging`用戶端使用。</span><span class="sxs-lookup"><span data-stu-id="543f3-463">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="543f3-464">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="543f3-464">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="543f3-465">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="543f3-465">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="543f3-466">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="543f3-466">Configure allowed transports</span></span>

<span data-ttu-id="543f3-467">SignalR 所使用的傳輸可以在`WithUrl`呼叫中設定（`withUrl`以 JavaScript 為限）。</span><span class="sxs-lookup"><span data-stu-id="543f3-467">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="543f3-468">值的`HttpTransportType`位 or 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-468">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="543f3-469">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-469">All transports are enabled by default.</span></span>

<span data-ttu-id="543f3-470">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="543f3-470">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="543f3-471">在 JavaScript 用戶端中，會透過在提供給`transport` `withUrl`的選項物件上設定欄位，來設定傳輸：</span><span class="sxs-lookup"><span data-stu-id="543f3-471">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="543f3-472">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-472">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="543f3-473">在 JAVA 用戶端中，會使用上的`withTransport`方法來選取傳輸`HttpHubConnectionBuilder`。</span><span class="sxs-lookup"><span data-stu-id="543f3-473">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="543f3-474">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-474">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="543f3-475">SignalR JAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="543f3-475">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="543f3-476">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="543f3-476">Configure bearer authentication</span></span>

<span data-ttu-id="543f3-477">若要提供驗證資料以及 SignalR 要求，請使用`AccessTokenProvider`選項（`accessTokenFactory`在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-477">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="543f3-478">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用類型為`Authorization`的`Bearer`標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="543f3-478">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="543f3-479">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-479">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="543f3-480">在這些情況下，存取權杖會以查詢字串值`access_token`的形式提供。</span><span class="sxs-lookup"><span data-stu-id="543f3-480">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="543f3-481">在 .NET 用戶端中， `AccessTokenProvider`可以使用中`WithUrl`的選項委派來指定選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-481">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="543f3-482">在 JavaScript 用戶端中，存取權杖是藉由在`accessTokenFactory` `withUrl`的選項物件上設定欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-482">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="543f3-483">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-483">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="543f3-484">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串>](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="543f3-484">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="543f3-485">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-485">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="543f3-486">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-486">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="543f3-487">設定 timeout 和 keep-alive 行為的其他選項可用於`HubConnection`物件本身：</span><span class="sxs-lookup"><span data-stu-id="543f3-487">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="543f3-488">.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-488">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="543f3-489">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-489">Option</span></span> | <span data-ttu-id="543f3-490">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-490">Default value</span></span> | <span data-ttu-id="543f3-491">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-491">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="543f3-492">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-492">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-493">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-493">Timeout for server activity.</span></span> <span data-ttu-id="543f3-494">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `Closed`並觸發`onclose`事件（在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-494">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="543f3-495">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-495">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-496">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-496">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="543f3-497">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-497">15 seconds</span></span> | <span data-ttu-id="543f3-498">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-498">Timeout for initial server handshake.</span></span> <span data-ttu-id="543f3-499">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`Closed`事件（`onclose`在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-499">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="543f3-500">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-500">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-501">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-501">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="543f3-502">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-502">15 seconds</span></span> | <span data-ttu-id="543f3-503">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-503">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-504">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-504">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-505">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-505">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="543f3-506">在 .NET 用戶端中，timeout 值會指定`TimeSpan`為值。</span><span class="sxs-lookup"><span data-stu-id="543f3-506">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="543f3-507">JavaScript</span><span class="sxs-lookup"><span data-stu-id="543f3-507">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="543f3-508">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-508">Option</span></span> | <span data-ttu-id="543f3-509">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-509">Default value</span></span> | <span data-ttu-id="543f3-510">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-510">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="543f3-511">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-511">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-512">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-512">Timeout for server activity.</span></span> <span data-ttu-id="543f3-513">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onclose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-513">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="543f3-514">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-514">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-515">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-515">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="543f3-516">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-516">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="543f3-517">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-517">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-518">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-518">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-519">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-519">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="543f3-520">Java</span><span class="sxs-lookup"><span data-stu-id="543f3-520">Java</span></span>](#tab/java)

| <span data-ttu-id="543f3-521">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-521">Option</span></span> | <span data-ttu-id="543f3-522">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-522">Default value</span></span> | <span data-ttu-id="543f3-523">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-523">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="543f3-524">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="543f3-524">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="543f3-525">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-525">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-526">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-526">Timeout for server activity.</span></span> <span data-ttu-id="543f3-527">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onClose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-527">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="543f3-528">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-528">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-529">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-529">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="543f3-530">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-530">15 seconds</span></span> | <span data-ttu-id="543f3-531">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-531">Timeout for initial server handshake.</span></span> <span data-ttu-id="543f3-532">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-532">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="543f3-533">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-533">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-534">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-534">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="543f3-535">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="543f3-535">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="543f3-536">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-536">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="543f3-537">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-537">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-538">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-538">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-539">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-539">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="543f3-540">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="543f3-540">Configure additional options</span></span>

<span data-ttu-id="543f3-541">您`WithUrl`可以在上`withUrl` `HubConnectionBuilder`的（JavaScript）方法中，或在 JAVA 用戶端`HttpHubConnectionBuilder`中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-541">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="543f3-542">.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-542">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="543f3-543">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-543">.NET Option</span></span> |  <span data-ttu-id="543f3-544">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-544">Default value</span></span> | <span data-ttu-id="543f3-545">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-545">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="543f3-546">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-546">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="543f3-547">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-547">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-548">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-548">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-549">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-549">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="543f3-550">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-550">Empty</span></span> | <span data-ttu-id="543f3-551">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="543f3-551">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="543f3-552">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-552">Empty</span></span> | <span data-ttu-id="543f3-553">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="543f3-553">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="543f3-554">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-554">Empty</span></span> | <span data-ttu-id="543f3-555">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-555">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="543f3-556">5 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-556">5 seconds</span></span> | <span data-ttu-id="543f3-557">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="543f3-557">WebSockets only.</span></span> <span data-ttu-id="543f3-558">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="543f3-558">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="543f3-559">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-559">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="543f3-560">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-560">Empty</span></span> | <span data-ttu-id="543f3-561">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="543f3-561">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="543f3-562">委派，可以用來設定或取代用來傳送`HttpMessageHandler` HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="543f3-562">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="543f3-563">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="543f3-563">Not used for WebSocket connections.</span></span> <span data-ttu-id="543f3-564">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="543f3-564">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="543f3-565">請修改該預設值的設定並加以傳回，或傳回新`HttpMessageHandler`的實例。</span><span class="sxs-lookup"><span data-stu-id="543f3-565">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="543f3-566">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="543f3-566">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="543f3-567">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="543f3-567">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="543f3-568">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-568">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="543f3-569">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="543f3-569">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="543f3-570">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-570">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="543f3-571">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="543f3-571">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="543f3-572">JavaScript</span><span class="sxs-lookup"><span data-stu-id="543f3-572">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="543f3-573">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-573">JavaScript Option</span></span> | <span data-ttu-id="543f3-574">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-574">Default Value</span></span> | <span data-ttu-id="543f3-575">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-575">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="543f3-576">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-576">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="543f3-577">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-577">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-578">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-578">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-579">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-579">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="543f3-580">Java</span><span class="sxs-lookup"><span data-stu-id="543f3-580">Java</span></span>](#tab/java)

| <span data-ttu-id="543f3-581">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-581">Java Option</span></span> | <span data-ttu-id="543f3-582">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-582">Default Value</span></span> | <span data-ttu-id="543f3-583">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-583">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="543f3-584">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-584">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="543f3-585">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-585">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-586">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-586">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-587">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-587">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="543f3-588">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="543f3-588">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="543f3-589">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-589">Empty</span></span> | <span data-ttu-id="543f3-590">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="543f3-590">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="543f3-591">在 .NET 用戶端中，這些選項可以由提供給`WithUrl`的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="543f3-591">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="543f3-592">在 JavaScript 用戶端中，可以在提供給`withUrl`的 javascript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-592">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="543f3-593">在 JAVA 用戶端中，您可以使用所`HttpHubConnectionBuilder`傳回之上的方法來設定這些選項。`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="543f3-593">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="543f3-594">其他資源</span><span class="sxs-lookup"><span data-stu-id="543f3-594">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="543f3-595">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="543f3-595">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="543f3-596">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="543f3-596">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="543f3-597">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-597">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="543f3-598">您可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，在伺服器上設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-598">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="543f3-599">`AddJsonProtocol`可以在中`Startup.ConfigureServices`的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="543f3-599">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="543f3-600">`AddJsonProtocol`方法會接受可接收`options`物件的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-600">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="543f3-601">該物件上的[PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是`System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions>物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-601">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="543f3-602">如需詳細資訊，請參閱[system.web 檔](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="543f3-602">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="543f3-603">例如，若要設定序列化程式不要變更屬性名稱的大小寫，而不是預設的 "camelCase" 名稱，請在中`Startup.ConfigureServices`使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="543f3-603">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    });
```

<span data-ttu-id="543f3-604">在 .NET 用戶端中，相同`AddJsonProtocol`的擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="543f3-604">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="543f3-605">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543f3-605">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="543f3-606">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-606">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="543f3-607">切換至 Newtonsoft. Json</span><span class="sxs-lookup"><span data-stu-id="543f3-607">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="543f3-608">如果您需要中`Newtonsoft.Json` `System.Text.Json`不支援的功能，請參閱[切換至 Newtonsoft。](xref:migration/22-to-30#switch-to-newtonsoftjson)</span><span class="sxs-lookup"><span data-stu-id="543f3-608">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="543f3-609">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="543f3-609">MessagePack serialization options</span></span>

<span data-ttu-id="543f3-610">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-610">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="543f3-611">如需詳細資訊，請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="543f3-611">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="543f3-612">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-612">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="543f3-613">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="543f3-613">Configure server options</span></span>

<span data-ttu-id="543f3-614">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-614">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="543f3-615">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-615">Option</span></span> | <span data-ttu-id="543f3-616">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-616">Default Value</span></span> | <span data-ttu-id="543f3-617">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-617">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="543f3-618">30 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-618">30 seconds</span></span> | <span data-ttu-id="543f3-619">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-619">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="543f3-620">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="543f3-620">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="543f3-621">建議的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="543f3-621">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="543f3-622">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-622">15 seconds</span></span> | <span data-ttu-id="543f3-623">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="543f3-623">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="543f3-624">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-624">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-625">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-625">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="543f3-626">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-626">15 seconds</span></span> | <span data-ttu-id="543f3-627">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="543f3-627">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="543f3-628">在變更`KeepAliveInterval`時，請`ServerTimeout` / `serverTimeoutInMilliseconds`變更用戶端上的設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-628">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="543f3-629">`ServerTimeout` /建議`serverTimeoutInMilliseconds`的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="543f3-629">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="543f3-630">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="543f3-630">All installed protocols</span></span> | <span data-ttu-id="543f3-631">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543f3-631">Protocols supported by this hub.</span></span> <span data-ttu-id="543f3-632">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543f3-632">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="543f3-633">如果`true`為，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-633">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="543f3-634">預設值為`false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="543f3-634">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="543f3-635">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="543f3-635">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="543f3-636">若達到此限制，則在伺服器處理資料流程專案之前，會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="543f3-636">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="543f3-637">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-637">32 KB</span></span> | <span data-ttu-id="543f3-638">單一傳入中樞訊息的大小上限。</span><span class="sxs-lookup"><span data-stu-id="543f3-638">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="543f3-639">您可以為所有中樞設定選項，方法是提供選項委派給`AddSignalR`中`Startup.ConfigureServices`的呼叫。</span><span class="sxs-lookup"><span data-stu-id="543f3-639">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="543f3-640">單一中樞的選項會覆寫中提供的全域`AddSignalR`選項，並可使用<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>下列方式進行設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-640">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="543f3-641">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="543f3-641">Advanced HTTP configuration options</span></span>

<span data-ttu-id="543f3-642">使用`HttpConnectionDispatcherOptions`來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-642">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="543f3-643">這些選項的設定方式是將委派傳遞至中`Startup.Configure`的[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) 。</span><span class="sxs-lookup"><span data-stu-id="543f3-643">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="543f3-644">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-644">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="543f3-645">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-645">Option</span></span> | <span data-ttu-id="543f3-646">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-646">Default Value</span></span> | <span data-ttu-id="543f3-647">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-647">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="543f3-648">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-648">32 KB</span></span> | <span data-ttu-id="543f3-649">在套用背壓之前，伺服器會緩衝的用戶端接收的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="543f3-649">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="543f3-650">增加此值可讓伺服器更快速地接收較大的訊息，而不需套用背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="543f3-650">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="543f3-651">從套用至中樞類別`Authorize`的屬性自動收集的資料。</span><span class="sxs-lookup"><span data-stu-id="543f3-651">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="543f3-652">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="543f3-652">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="543f3-653">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-653">32 KB</span></span> | <span data-ttu-id="543f3-654">在觀察到背壓之前，伺服器會緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="543f3-654">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="543f3-655">增加此值可讓伺服器更快速地緩衝較大的訊息，而不需等待背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="543f3-655">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="543f3-656">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="543f3-656">All Transports are enabled.</span></span> | <span data-ttu-id="543f3-657">`HttpTransportType`值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-657">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="543f3-658">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="543f3-658">See below.</span></span> | <span data-ttu-id="543f3-659">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-659">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="543f3-660">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="543f3-660">See below.</span></span> | <span data-ttu-id="543f3-661">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-661">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="543f3-662">長輪詢傳輸具有其他選項，可以使用`LongPolling`屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-662">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="543f3-663">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-663">Option</span></span> | <span data-ttu-id="543f3-664">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-664">Default Value</span></span> | <span data-ttu-id="543f3-665">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-665">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="543f3-666">90秒</span><span class="sxs-lookup"><span data-stu-id="543f3-666">90 seconds</span></span> | <span data-ttu-id="543f3-667">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="543f3-667">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="543f3-668">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="543f3-668">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="543f3-669">WebSocket 傳輸具有可使用`WebSockets`屬性來設定的其他選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-669">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="543f3-670">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-670">Option</span></span> | <span data-ttu-id="543f3-671">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-671">Default Value</span></span> | <span data-ttu-id="543f3-672">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-672">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="543f3-673">5 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-673">5 seconds</span></span> | <span data-ttu-id="543f3-674">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="543f3-674">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="543f3-675">可以用來將`Sec-WebSocket-Protocol`標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-675">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="543f3-676">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="543f3-676">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="543f3-677">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="543f3-677">Configure client options</span></span>

<span data-ttu-id="543f3-678">您可以在`HubConnectionBuilder`類型上設定用戶端選項（可在 .Net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="543f3-678">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="543f3-679">它也可在 JAVA 用戶端中使用，但`HttpHubConnectionBuilder`子類別則包含產生器設定選項，以及`HubConnection`本身。</span><span class="sxs-lookup"><span data-stu-id="543f3-679">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="543f3-680">設定記錄</span><span class="sxs-lookup"><span data-stu-id="543f3-680">Configure logging</span></span>

<span data-ttu-id="543f3-681">使用`ConfigureLogging`方法，在 .Net 用戶端中設定記錄。</span><span class="sxs-lookup"><span data-stu-id="543f3-681">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="543f3-682">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="543f3-682">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="543f3-683">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="543f3-683">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="543f3-684">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="543f3-684">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="543f3-685">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="543f3-685">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="543f3-686">例如，若要啟用主控台記錄，請安裝`Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="543f3-686">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="543f3-687">呼叫`AddConsole`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543f3-687">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="543f3-688">在 JavaScript 用戶端中，有`configureLogging`類似的方法存在。</span><span class="sxs-lookup"><span data-stu-id="543f3-688">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="543f3-689">提供`LogLevel`值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-689">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="543f3-690">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="543f3-690">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="543f3-691">您也可以`LogLevel`提供代表記錄層級名稱的`string`值，而不是值。</span><span class="sxs-lookup"><span data-stu-id="543f3-691">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="543f3-692">在您無法存取常數的`LogLevel`環境中設定 SignalR 記錄時，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="543f3-692">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="543f3-693">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-693">The following table lists the available log levels.</span></span> <span data-ttu-id="543f3-694">您所提供的值`configureLogging` ，用來設定將記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-694">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="543f3-695">記錄在此層級的訊息，**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="543f3-695">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="543f3-696">String</span><span class="sxs-lookup"><span data-stu-id="543f3-696">String</span></span>                      | <span data-ttu-id="543f3-697">LogLevel</span><span class="sxs-lookup"><span data-stu-id="543f3-697">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="543f3-698">`info` **或** `information`</span><span class="sxs-lookup"><span data-stu-id="543f3-698">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="543f3-699">`warn` **或** `warning`</span><span class="sxs-lookup"><span data-stu-id="543f3-699">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="543f3-700">若要完全停用記錄`signalR.LogLevel.None` ，請`configureLogging`在方法中指定。</span><span class="sxs-lookup"><span data-stu-id="543f3-700">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="543f3-701">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="543f3-701">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="543f3-702">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="543f3-702">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="543f3-703">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="543f3-703">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="543f3-704">下列程式碼片段示範如何搭配 SignalR JAVA `java.util.logging`用戶端使用。</span><span class="sxs-lookup"><span data-stu-id="543f3-704">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="543f3-705">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="543f3-705">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="543f3-706">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="543f3-706">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="543f3-707">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="543f3-707">Configure allowed transports</span></span>

<span data-ttu-id="543f3-708">SignalR 所使用的傳輸可以在`WithUrl`呼叫中設定（`withUrl`以 JavaScript 為限）。</span><span class="sxs-lookup"><span data-stu-id="543f3-708">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="543f3-709">值的`HttpTransportType`位 or 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-709">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="543f3-710">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-710">All transports are enabled by default.</span></span>

<span data-ttu-id="543f3-711">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="543f3-711">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="543f3-712">在 JavaScript 用戶端中，會透過在提供給`transport` `withUrl`的選項物件上設定欄位，來設定傳輸：</span><span class="sxs-lookup"><span data-stu-id="543f3-712">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="543f3-713">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-713">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="543f3-714">在 JAVA 用戶端中，會使用上的`withTransport`方法來選取傳輸`HttpHubConnectionBuilder`。</span><span class="sxs-lookup"><span data-stu-id="543f3-714">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="543f3-715">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-715">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="543f3-716">SignalR JAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="543f3-716">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="543f3-717">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="543f3-717">Configure bearer authentication</span></span>

<span data-ttu-id="543f3-718">若要提供驗證資料以及 SignalR 要求，請使用`AccessTokenProvider`選項（`accessTokenFactory`在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-718">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="543f3-719">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用類型為`Authorization`的`Bearer`標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="543f3-719">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="543f3-720">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-720">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="543f3-721">在這些情況下，存取權杖會以查詢字串值`access_token`的形式提供。</span><span class="sxs-lookup"><span data-stu-id="543f3-721">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="543f3-722">在 .NET 用戶端中， `AccessTokenProvider`可以使用中`WithUrl`的選項委派來指定選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-722">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="543f3-723">在 JavaScript 用戶端中，存取權杖是藉由在`accessTokenFactory` `withUrl`的選項物件上設定欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-723">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="543f3-724">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-724">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="543f3-725">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串>](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="543f3-725">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="543f3-726">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-726">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="543f3-727">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-727">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="543f3-728">設定 timeout 和 keep-alive 行為的其他選項可用於`HubConnection`物件本身：</span><span class="sxs-lookup"><span data-stu-id="543f3-728">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="543f3-729">.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-729">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="543f3-730">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-730">Option</span></span> | <span data-ttu-id="543f3-731">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-731">Default value</span></span> | <span data-ttu-id="543f3-732">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-732">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="543f3-733">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-733">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-734">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-734">Timeout for server activity.</span></span> <span data-ttu-id="543f3-735">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `Closed`並觸發`onclose`事件（在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-735">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="543f3-736">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-736">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-737">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-737">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="543f3-738">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-738">15 seconds</span></span> | <span data-ttu-id="543f3-739">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-739">Timeout for initial server handshake.</span></span> <span data-ttu-id="543f3-740">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`Closed`事件（`onclose`在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-740">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="543f3-741">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-741">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-742">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-742">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="543f3-743">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-743">15 seconds</span></span> | <span data-ttu-id="543f3-744">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-744">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-745">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-745">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-746">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-746">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="543f3-747">在 .NET 用戶端中，timeout 值會指定`TimeSpan`為值。</span><span class="sxs-lookup"><span data-stu-id="543f3-747">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="543f3-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="543f3-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="543f3-749">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-749">Option</span></span> | <span data-ttu-id="543f3-750">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-750">Default value</span></span> | <span data-ttu-id="543f3-751">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-751">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="543f3-752">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-752">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-753">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-753">Timeout for server activity.</span></span> <span data-ttu-id="543f3-754">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onclose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-754">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="543f3-755">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-755">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-756">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-756">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="543f3-757">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-757">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="543f3-758">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-758">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-759">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-759">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-760">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-760">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="543f3-761">Java</span><span class="sxs-lookup"><span data-stu-id="543f3-761">Java</span></span>](#tab/java)

| <span data-ttu-id="543f3-762">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-762">Option</span></span> | <span data-ttu-id="543f3-763">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-763">Default value</span></span> | <span data-ttu-id="543f3-764">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-764">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="543f3-765">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="543f3-765">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="543f3-766">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-766">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-767">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-767">Timeout for server activity.</span></span> <span data-ttu-id="543f3-768">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onClose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-768">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="543f3-769">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-769">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-770">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-770">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="543f3-771">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-771">15 seconds</span></span> | <span data-ttu-id="543f3-772">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-772">Timeout for initial server handshake.</span></span> <span data-ttu-id="543f3-773">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-773">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="543f3-774">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-774">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-775">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-775">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="543f3-776">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="543f3-776">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="543f3-777">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-777">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="543f3-778">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-778">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-779">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-779">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-780">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-780">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="543f3-781">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="543f3-781">Configure additional options</span></span>

<span data-ttu-id="543f3-782">您`WithUrl`可以在上`withUrl` `HubConnectionBuilder`的（JavaScript）方法中，或在 JAVA 用戶端`HttpHubConnectionBuilder`中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-782">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="543f3-783">.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-783">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="543f3-784">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-784">.NET Option</span></span> |  <span data-ttu-id="543f3-785">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-785">Default value</span></span> | <span data-ttu-id="543f3-786">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-786">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="543f3-787">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-787">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="543f3-788">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-788">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-789">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-789">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-790">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-790">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="543f3-791">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-791">Empty</span></span> | <span data-ttu-id="543f3-792">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="543f3-792">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="543f3-793">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-793">Empty</span></span> | <span data-ttu-id="543f3-794">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="543f3-794">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="543f3-795">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-795">Empty</span></span> | <span data-ttu-id="543f3-796">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-796">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="543f3-797">5 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-797">5 seconds</span></span> | <span data-ttu-id="543f3-798">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="543f3-798">WebSockets only.</span></span> <span data-ttu-id="543f3-799">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="543f3-799">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="543f3-800">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-800">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="543f3-801">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-801">Empty</span></span> | <span data-ttu-id="543f3-802">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="543f3-802">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="543f3-803">委派，可以用來設定或取代用來傳送`HttpMessageHandler` HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="543f3-803">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="543f3-804">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="543f3-804">Not used for WebSocket connections.</span></span> <span data-ttu-id="543f3-805">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="543f3-805">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="543f3-806">請修改該預設值的設定並加以傳回，或傳回新`HttpMessageHandler`的實例。</span><span class="sxs-lookup"><span data-stu-id="543f3-806">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="543f3-807">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="543f3-807">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="543f3-808">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="543f3-808">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="543f3-809">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-809">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="543f3-810">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="543f3-810">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="543f3-811">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-811">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="543f3-812">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="543f3-812">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="543f3-813">JavaScript</span><span class="sxs-lookup"><span data-stu-id="543f3-813">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="543f3-814">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-814">JavaScript Option</span></span> | <span data-ttu-id="543f3-815">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-815">Default Value</span></span> | <span data-ttu-id="543f3-816">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-816">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="543f3-817">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-817">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="543f3-818">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-818">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-819">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-819">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-820">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-820">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="543f3-821">Java</span><span class="sxs-lookup"><span data-stu-id="543f3-821">Java</span></span>](#tab/java)

| <span data-ttu-id="543f3-822">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-822">Java Option</span></span> | <span data-ttu-id="543f3-823">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-823">Default Value</span></span> | <span data-ttu-id="543f3-824">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-824">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="543f3-825">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-825">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="543f3-826">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-826">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-827">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-827">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-828">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-828">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="543f3-829">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="543f3-829">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="543f3-830">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-830">Empty</span></span> | <span data-ttu-id="543f3-831">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="543f3-831">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="543f3-832">在 .NET 用戶端中，這些選項可以由提供給`WithUrl`的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="543f3-832">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="543f3-833">在 JavaScript 用戶端中，可以在提供給`withUrl`的 javascript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-833">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="543f3-834">在 JAVA 用戶端中，您可以使用所`HttpHubConnectionBuilder`傳回之上的方法來設定這些選項。`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="543f3-834">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="543f3-835">其他資源</span><span class="sxs-lookup"><span data-stu-id="543f3-835">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="543f3-836">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="543f3-836">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="543f3-837">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="543f3-837">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="543f3-838">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-838">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="543f3-839">您可以在伺服器上使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法來設定 JSON 序列化，這可在您`Startup.ConfigureServices`的方法中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="543f3-839">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="543f3-840">`AddJsonProtocol`方法會接受可接收`options`物件的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-840">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="543f3-841">該物件上的[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性是 JSON.NET `JsonSerializerSettings`物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-841">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="543f3-842">如需詳細資訊，請參閱[JSON.NET 檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="543f3-842">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="543f3-843">例如，若要設定序列化程式使用 "PascalCase" 屬性名稱，而不是預設的 "camelCase" 名稱，請在中`Startup.ConfigureServices`使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="543f3-843">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="543f3-844">在 .NET 用戶端中，相同`AddJsonProtocol`的擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="543f3-844">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="543f3-845">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543f3-845">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="543f3-846">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-846">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="543f3-847">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="543f3-847">MessagePack serialization options</span></span>

<span data-ttu-id="543f3-848">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-848">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="543f3-849">如需詳細資訊，請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="543f3-849">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="543f3-850">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-850">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="543f3-851">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="543f3-851">Configure server options</span></span>

<span data-ttu-id="543f3-852">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-852">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="543f3-853">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-853">Option</span></span> | <span data-ttu-id="543f3-854">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-854">Default Value</span></span> | <span data-ttu-id="543f3-855">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-855">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="543f3-856">30 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-856">30 seconds</span></span> | <span data-ttu-id="543f3-857">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-857">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="543f3-858">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="543f3-858">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="543f3-859">建議的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="543f3-859">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="543f3-860">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-860">15 seconds</span></span> | <span data-ttu-id="543f3-861">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="543f3-861">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="543f3-862">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-862">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-863">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-863">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="543f3-864">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-864">15 seconds</span></span> | <span data-ttu-id="543f3-865">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="543f3-865">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="543f3-866">在變更`KeepAliveInterval`時，請`ServerTimeout` / `serverTimeoutInMilliseconds`變更用戶端上的設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-866">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="543f3-867">`ServerTimeout` /建議`serverTimeoutInMilliseconds`的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="543f3-867">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="543f3-868">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="543f3-868">All installed protocols</span></span> | <span data-ttu-id="543f3-869">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543f3-869">Protocols supported by this hub.</span></span> <span data-ttu-id="543f3-870">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543f3-870">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="543f3-871">如果`true`為，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-871">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="543f3-872">預設值為`false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="543f3-872">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="543f3-873">您可以為所有中樞設定選項，方法是提供選項委派給`AddSignalR`中`Startup.ConfigureServices`的呼叫。</span><span class="sxs-lookup"><span data-stu-id="543f3-873">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="543f3-874">單一中樞的選項會覆寫中提供的全域`AddSignalR`選項，並可使用<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>下列方式進行設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-874">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="543f3-875">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="543f3-875">Advanced HTTP configuration options</span></span>

<span data-ttu-id="543f3-876">使用`HttpConnectionDispatcherOptions`來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-876">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="543f3-877">這些選項的設定方式是將委派傳遞至中`Startup.Configure`的[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) 。</span><span class="sxs-lookup"><span data-stu-id="543f3-877">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="543f3-878">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-878">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="543f3-879">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-879">Option</span></span> | <span data-ttu-id="543f3-880">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-880">Default Value</span></span> | <span data-ttu-id="543f3-881">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-881">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="543f3-882">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-882">32 KB</span></span> | <span data-ttu-id="543f3-883">從用戶端接收伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="543f3-883">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="543f3-884">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="543f3-884">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="543f3-885">從套用至中樞類別`Authorize`的屬性自動收集的資料。</span><span class="sxs-lookup"><span data-stu-id="543f3-885">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="543f3-886">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="543f3-886">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="543f3-887">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-887">32 KB</span></span> | <span data-ttu-id="543f3-888">由伺服器所緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="543f3-888">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="543f3-889">增加這個值可讓伺服器傳送較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="543f3-889">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="543f3-890">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="543f3-890">All Transports are enabled.</span></span> | <span data-ttu-id="543f3-891">`HttpTransportType`值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-891">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="543f3-892">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="543f3-892">See below.</span></span> | <span data-ttu-id="543f3-893">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-893">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="543f3-894">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="543f3-894">See below.</span></span> | <span data-ttu-id="543f3-895">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-895">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="543f3-896">長輪詢傳輸具有其他選項，可以使用`LongPolling`屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-896">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="543f3-897">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-897">Option</span></span> | <span data-ttu-id="543f3-898">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-898">Default Value</span></span> | <span data-ttu-id="543f3-899">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-899">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="543f3-900">90秒</span><span class="sxs-lookup"><span data-stu-id="543f3-900">90 seconds</span></span> | <span data-ttu-id="543f3-901">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="543f3-901">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="543f3-902">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="543f3-902">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="543f3-903">WebSocket 傳輸具有可使用`WebSockets`屬性來設定的其他選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-903">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="543f3-904">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-904">Option</span></span> | <span data-ttu-id="543f3-905">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-905">Default Value</span></span> | <span data-ttu-id="543f3-906">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-906">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="543f3-907">5 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-907">5 seconds</span></span> | <span data-ttu-id="543f3-908">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="543f3-908">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="543f3-909">可以用來將`Sec-WebSocket-Protocol`標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-909">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="543f3-910">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="543f3-910">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="543f3-911">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="543f3-911">Configure client options</span></span>

<span data-ttu-id="543f3-912">您可以在`HubConnectionBuilder`類型上設定用戶端選項（可在 .Net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="543f3-912">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="543f3-913">它也可在 JAVA 用戶端中使用，但`HttpHubConnectionBuilder`子類別則包含產生器設定選項，以及`HubConnection`本身。</span><span class="sxs-lookup"><span data-stu-id="543f3-913">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="543f3-914">設定記錄</span><span class="sxs-lookup"><span data-stu-id="543f3-914">Configure logging</span></span>

<span data-ttu-id="543f3-915">使用`ConfigureLogging`方法，在 .Net 用戶端中設定記錄。</span><span class="sxs-lookup"><span data-stu-id="543f3-915">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="543f3-916">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="543f3-916">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="543f3-917">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="543f3-917">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="543f3-918">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="543f3-918">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="543f3-919">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="543f3-919">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="543f3-920">例如，若要啟用主控台記錄，請安裝`Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="543f3-920">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="543f3-921">呼叫`AddConsole`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543f3-921">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="543f3-922">在 JavaScript 用戶端中，有`configureLogging`類似的方法存在。</span><span class="sxs-lookup"><span data-stu-id="543f3-922">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="543f3-923">提供`LogLevel`值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-923">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="543f3-924">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="543f3-924">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="543f3-925">若要完全停用記錄`signalR.LogLevel.None` ，請`configureLogging`在方法中指定。</span><span class="sxs-lookup"><span data-stu-id="543f3-925">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="543f3-926">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="543f3-926">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="543f3-927">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="543f3-927">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="543f3-928">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="543f3-928">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="543f3-929">下列程式碼片段示範如何搭配 SignalR JAVA `java.util.logging`用戶端使用。</span><span class="sxs-lookup"><span data-stu-id="543f3-929">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="543f3-930">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="543f3-930">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="543f3-931">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="543f3-931">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="543f3-932">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="543f3-932">Configure allowed transports</span></span>

<span data-ttu-id="543f3-933">SignalR 所使用的傳輸可以在`WithUrl`呼叫中設定（`withUrl`以 JavaScript 為限）。</span><span class="sxs-lookup"><span data-stu-id="543f3-933">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="543f3-934">值的`HttpTransportType`位 or 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-934">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="543f3-935">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-935">All transports are enabled by default.</span></span>

<span data-ttu-id="543f3-936">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="543f3-936">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="543f3-937">在 JavaScript 用戶端中，會透過在提供給`transport` `withUrl`的選項物件上設定欄位，來設定傳輸：</span><span class="sxs-lookup"><span data-stu-id="543f3-937">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="543f3-938">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-938">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="543f3-939">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="543f3-939">Configure bearer authentication</span></span>

<span data-ttu-id="543f3-940">若要提供驗證資料以及 SignalR 要求，請使用`AccessTokenProvider`選項（`accessTokenFactory`在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-940">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="543f3-941">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用類型為`Authorization`的`Bearer`標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="543f3-941">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="543f3-942">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-942">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="543f3-943">在這些情況下，存取權杖會以查詢字串值`access_token`的形式提供。</span><span class="sxs-lookup"><span data-stu-id="543f3-943">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="543f3-944">在 .NET 用戶端中， `AccessTokenProvider`可以使用中`WithUrl`的選項委派來指定選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-944">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="543f3-945">在 JavaScript 用戶端中，存取權杖是藉由在`accessTokenFactory` `withUrl`的選項物件上設定欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-945">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="543f3-946">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-946">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="543f3-947">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串>](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="543f3-947">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="543f3-948">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-948">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="543f3-949">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-949">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="543f3-950">設定 timeout 和 keep-alive 行為的其他選項可用於`HubConnection`物件本身：</span><span class="sxs-lookup"><span data-stu-id="543f3-950">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="543f3-951">.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-951">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="543f3-952">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-952">Option</span></span> | <span data-ttu-id="543f3-953">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-953">Default value</span></span> | <span data-ttu-id="543f3-954">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-954">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="543f3-955">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-955">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-956">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-956">Timeout for server activity.</span></span> <span data-ttu-id="543f3-957">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `Closed`並觸發`onclose`事件（在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-957">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="543f3-958">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-958">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-959">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-959">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="543f3-960">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-960">15 seconds</span></span> | <span data-ttu-id="543f3-961">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-961">Timeout for initial server handshake.</span></span> <span data-ttu-id="543f3-962">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`Closed`事件（`onclose`在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-962">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="543f3-963">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-963">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-964">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-964">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="543f3-965">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-965">15 seconds</span></span> | <span data-ttu-id="543f3-966">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-966">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-967">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-967">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-968">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-968">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="543f3-969">在 .NET 用戶端中，timeout 值會指定`TimeSpan`為值。</span><span class="sxs-lookup"><span data-stu-id="543f3-969">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="543f3-970">JavaScript</span><span class="sxs-lookup"><span data-stu-id="543f3-970">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="543f3-971">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-971">Option</span></span> | <span data-ttu-id="543f3-972">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-972">Default value</span></span> | <span data-ttu-id="543f3-973">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-973">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="543f3-974">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-974">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-975">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-975">Timeout for server activity.</span></span> <span data-ttu-id="543f3-976">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onclose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-976">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="543f3-977">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-977">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-978">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-978">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="543f3-979">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-979">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="543f3-980">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-980">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-981">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-981">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-982">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-982">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="543f3-983">Java</span><span class="sxs-lookup"><span data-stu-id="543f3-983">Java</span></span>](#tab/java)

| <span data-ttu-id="543f3-984">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-984">Option</span></span> | <span data-ttu-id="543f3-985">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-985">Default value</span></span> | <span data-ttu-id="543f3-986">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-986">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="543f3-987">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="543f3-987">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="543f3-988">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-988">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-989">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-989">Timeout for server activity.</span></span> <span data-ttu-id="543f3-990">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onClose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-990">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="543f3-991">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-991">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-992">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-992">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="543f3-993">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-993">15 seconds</span></span> | <span data-ttu-id="543f3-994">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-994">Timeout for initial server handshake.</span></span> <span data-ttu-id="543f3-995">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-995">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="543f3-996">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-996">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-997">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-997">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="543f3-998">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="543f3-998">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="543f3-999">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-999">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="543f3-1000">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="543f3-1000">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="543f3-1001">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="543f3-1001">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="543f3-1002">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-1002">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="543f3-1003">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1003">Configure additional options</span></span>

<span data-ttu-id="543f3-1004">您`WithUrl`可以在上`withUrl` `HubConnectionBuilder`的（JavaScript）方法中，或在 JAVA 用戶端`HttpHubConnectionBuilder`中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-1004">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="543f3-1005">.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-1005">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="543f3-1006">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1006">.NET Option</span></span> |  <span data-ttu-id="543f3-1007">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1007">Default value</span></span> | <span data-ttu-id="543f3-1008">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-1008">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="543f3-1009">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-1009">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="543f3-1010">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-1010">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-1011">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-1011">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-1012">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1012">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="543f3-1013">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-1013">Empty</span></span> | <span data-ttu-id="543f3-1014">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="543f3-1014">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="543f3-1015">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-1015">Empty</span></span> | <span data-ttu-id="543f3-1016">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="543f3-1016">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="543f3-1017">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-1017">Empty</span></span> | <span data-ttu-id="543f3-1018">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-1018">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="543f3-1019">5 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-1019">5 seconds</span></span> | <span data-ttu-id="543f3-1020">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="543f3-1020">WebSockets only.</span></span> <span data-ttu-id="543f3-1021">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="543f3-1021">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="543f3-1022">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-1022">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="543f3-1023">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-1023">Empty</span></span> | <span data-ttu-id="543f3-1024">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="543f3-1024">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="543f3-1025">委派，可以用來設定或取代用來傳送`HttpMessageHandler` HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="543f3-1025">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="543f3-1026">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="543f3-1026">Not used for WebSocket connections.</span></span> <span data-ttu-id="543f3-1027">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="543f3-1027">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="543f3-1028">請修改該預設值的設定並加以傳回，或傳回新`HttpMessageHandler`的實例。</span><span class="sxs-lookup"><span data-stu-id="543f3-1028">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="543f3-1029">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="543f3-1029">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="543f3-1030">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="543f3-1030">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="543f3-1031">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-1031">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="543f3-1032">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="543f3-1032">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="543f3-1033">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-1033">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="543f3-1034">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="543f3-1034">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="543f3-1035">JavaScript</span><span class="sxs-lookup"><span data-stu-id="543f3-1035">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="543f3-1036">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1036">JavaScript Option</span></span> | <span data-ttu-id="543f3-1037">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1037">Default Value</span></span> | <span data-ttu-id="543f3-1038">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-1038">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="543f3-1039">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-1039">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="543f3-1040">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-1040">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-1041">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-1041">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-1042">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1042">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="543f3-1043">Java</span><span class="sxs-lookup"><span data-stu-id="543f3-1043">Java</span></span>](#tab/java)

| <span data-ttu-id="543f3-1044">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1044">Java Option</span></span> | <span data-ttu-id="543f3-1045">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1045">Default Value</span></span> | <span data-ttu-id="543f3-1046">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-1046">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="543f3-1047">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-1047">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="543f3-1048">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-1048">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-1049">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-1049">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-1050">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1050">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="543f3-1051">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="543f3-1051">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="543f3-1052">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-1052">Empty</span></span> | <span data-ttu-id="543f3-1053">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="543f3-1053">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="543f3-1054">在 .NET 用戶端中，這些選項可以由提供給`WithUrl`的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="543f3-1054">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="543f3-1055">在 JavaScript 用戶端中，可以在提供給`withUrl`的 javascript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-1055">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="543f3-1056">在 JAVA 用戶端中，您可以使用所`HttpHubConnectionBuilder`傳回之上的方法來設定這些選項。`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="543f3-1056">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="543f3-1057">其他資源</span><span class="sxs-lookup"><span data-stu-id="543f3-1057">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="543f3-1058">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1058">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="543f3-1059">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="543f3-1059">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="543f3-1060">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-1060">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="543f3-1061">您可以在伺服器上使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法來設定 JSON 序列化，這可在您`Startup.ConfigureServices`的方法中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="543f3-1061">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="543f3-1062">`AddJsonProtocol`方法會接受可接收`options`物件的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-1062">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="543f3-1063">該物件上的[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性是 JSON.NET `JsonSerializerSettings`物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-1063">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="543f3-1064">如需詳細資訊，請參閱[JSON.NET 檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="543f3-1064">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="543f3-1065">例如，若要設定序列化程式使用 "PascalCase" 屬性名稱，而不是預設的 "camelCase" 名稱，請在中`Startup.ConfigureServices`使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="543f3-1065">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="543f3-1066">在 .NET 用戶端中，相同`AddJsonProtocol`的擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="543f3-1066">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="543f3-1067">必須`Microsoft.Extensions.DependencyInjection`匯入命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543f3-1067">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="543f3-1068">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-1068">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="543f3-1069">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1069">MessagePack serialization options</span></span>

<span data-ttu-id="543f3-1070">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1070">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="543f3-1071">如需詳細資訊，請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="543f3-1071">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="543f3-1072">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="543f3-1072">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="543f3-1073">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1073">Configure server options</span></span>

<span data-ttu-id="543f3-1074">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-1074">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="543f3-1075">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1075">Option</span></span> | <span data-ttu-id="543f3-1076">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1076">Default Value</span></span> | <span data-ttu-id="543f3-1077">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-1077">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="543f3-1078">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-1078">15 seconds</span></span> | <span data-ttu-id="543f3-1079">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="543f3-1079">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="543f3-1080">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1080">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-1081">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-1081">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="543f3-1082">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-1082">15 seconds</span></span> | <span data-ttu-id="543f3-1083">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="543f3-1083">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="543f3-1084">在變更`KeepAliveInterval`時，請`ServerTimeout` / `serverTimeoutInMilliseconds`變更用戶端上的設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1084">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="543f3-1085">`ServerTimeout` /建議`serverTimeoutInMilliseconds`的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="543f3-1085">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="543f3-1086">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="543f3-1086">All installed protocols</span></span> | <span data-ttu-id="543f3-1087">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1087">Protocols supported by this hub.</span></span> <span data-ttu-id="543f3-1088">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1088">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="543f3-1089">如果`true`為，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-1089">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="543f3-1090">預設值為`false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="543f3-1090">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="543f3-1091">您可以為所有中樞設定選項，方法是提供選項委派給`AddSignalR`中`Startup.ConfigureServices`的呼叫。</span><span class="sxs-lookup"><span data-stu-id="543f3-1091">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="543f3-1092">單一中樞的選項會覆寫中提供的全域`AddSignalR`選項，並可使用<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>下列方式進行設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-1092">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="543f3-1093">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1093">Advanced HTTP configuration options</span></span>

<span data-ttu-id="543f3-1094">使用`HttpConnectionDispatcherOptions`來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1094">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="543f3-1095">這些選項的設定方式是將委派傳遞至中`Startup.Configure`的[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) 。</span><span class="sxs-lookup"><span data-stu-id="543f3-1095">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="543f3-1096">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-1096">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="543f3-1097">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1097">Option</span></span> | <span data-ttu-id="543f3-1098">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1098">Default Value</span></span> | <span data-ttu-id="543f3-1099">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-1099">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="543f3-1100">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-1100">32 KB</span></span> | <span data-ttu-id="543f3-1101">從用戶端接收伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="543f3-1101">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="543f3-1102">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="543f3-1102">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="543f3-1103">從套用至中樞類別`Authorize`的屬性自動收集的資料。</span><span class="sxs-lookup"><span data-stu-id="543f3-1103">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="543f3-1104">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="543f3-1104">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="543f3-1105">32 KB</span><span class="sxs-lookup"><span data-stu-id="543f3-1105">32 KB</span></span> | <span data-ttu-id="543f3-1106">由伺服器所緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="543f3-1106">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="543f3-1107">增加這個值可讓伺服器傳送較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="543f3-1107">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="543f3-1108">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="543f3-1108">All Transports are enabled.</span></span> | <span data-ttu-id="543f3-1109">`HttpTransportType`值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-1109">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="543f3-1110">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="543f3-1110">See below.</span></span> | <span data-ttu-id="543f3-1111">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-1111">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="543f3-1112">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="543f3-1112">See below.</span></span> | <span data-ttu-id="543f3-1113">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="543f3-1113">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="543f3-1114">長輪詢傳輸具有其他選項，可以使用`LongPolling`屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-1114">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="543f3-1115">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1115">Option</span></span> | <span data-ttu-id="543f3-1116">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1116">Default Value</span></span> | <span data-ttu-id="543f3-1117">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-1117">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="543f3-1118">90秒</span><span class="sxs-lookup"><span data-stu-id="543f3-1118">90 seconds</span></span> | <span data-ttu-id="543f3-1119">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="543f3-1119">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="543f3-1120">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="543f3-1120">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="543f3-1121">WebSocket 傳輸具有可使用`WebSockets`屬性來設定的其他選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-1121">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="543f3-1122">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1122">Option</span></span> | <span data-ttu-id="543f3-1123">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1123">Default Value</span></span> | <span data-ttu-id="543f3-1124">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-1124">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="543f3-1125">5 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-1125">5 seconds</span></span> | <span data-ttu-id="543f3-1126">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="543f3-1126">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="543f3-1127">可以用來將`Sec-WebSocket-Protocol`標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-1127">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="543f3-1128">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="543f3-1128">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="543f3-1129">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1129">Configure client options</span></span>

<span data-ttu-id="543f3-1130">您可以在`HubConnectionBuilder`類型上設定用戶端選項（可在 .Net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="543f3-1130">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="543f3-1131">它也可在 JAVA 用戶端中使用，但`HttpHubConnectionBuilder`子類別則包含產生器設定選項，以及`HubConnection`本身。</span><span class="sxs-lookup"><span data-stu-id="543f3-1131">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="543f3-1132">設定記錄</span><span class="sxs-lookup"><span data-stu-id="543f3-1132">Configure logging</span></span>

<span data-ttu-id="543f3-1133">使用`ConfigureLogging`方法，在 .Net 用戶端中設定記錄。</span><span class="sxs-lookup"><span data-stu-id="543f3-1133">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="543f3-1134">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="543f3-1134">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="543f3-1135">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="543f3-1135">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="543f3-1136">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="543f3-1136">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="543f3-1137">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="543f3-1137">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="543f3-1138">例如，若要啟用主控台記錄，請安裝`Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="543f3-1138">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="543f3-1139">呼叫`AddConsole`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543f3-1139">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="543f3-1140">在 JavaScript 用戶端中，有`configureLogging`類似的方法存在。</span><span class="sxs-lookup"><span data-stu-id="543f3-1140">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="543f3-1141">提供`LogLevel`值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="543f3-1141">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="543f3-1142">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="543f3-1142">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="543f3-1143">若要完全停用記錄`signalR.LogLevel.None` ，請`configureLogging`在方法中指定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1143">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="543f3-1144">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="543f3-1144">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="543f3-1145">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="543f3-1145">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="543f3-1146">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="543f3-1146">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="543f3-1147">下列程式碼片段示範如何搭配 SignalR JAVA `java.util.logging`用戶端使用。</span><span class="sxs-lookup"><span data-stu-id="543f3-1147">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="543f3-1148">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="543f3-1148">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="543f3-1149">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="543f3-1149">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="543f3-1150">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="543f3-1150">Configure allowed transports</span></span>

<span data-ttu-id="543f3-1151">SignalR 所使用的傳輸可以在`WithUrl`呼叫中設定（`withUrl`以 JavaScript 為限）。</span><span class="sxs-lookup"><span data-stu-id="543f3-1151">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="543f3-1152">值的`HttpTransportType`位 or 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-1152">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="543f3-1153">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="543f3-1153">All transports are enabled by default.</span></span>

<span data-ttu-id="543f3-1154">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="543f3-1154">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="543f3-1155">在 JavaScript 用戶端中，會透過在提供給`transport` `withUrl`的選項物件上設定欄位，來設定傳輸：</span><span class="sxs-lookup"><span data-stu-id="543f3-1155">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="543f3-1156">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="543f3-1156">Configure bearer authentication</span></span>

<span data-ttu-id="543f3-1157">若要提供驗證資料以及 SignalR 要求，請使用`AccessTokenProvider`選項（`accessTokenFactory`在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-1157">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="543f3-1158">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用類型為`Authorization`的`Bearer`標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="543f3-1158">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="543f3-1159">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-1159">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="543f3-1160">在這些情況下，存取權杖會以查詢字串值`access_token`的形式提供。</span><span class="sxs-lookup"><span data-stu-id="543f3-1160">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="543f3-1161">在 .NET 用戶端中， `AccessTokenProvider`可以使用中`WithUrl`的選項委派來指定選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-1161">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="543f3-1162">在 JavaScript 用戶端中，存取權杖是藉由在`accessTokenFactory` `withUrl`的選項物件上設定欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="543f3-1162">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="543f3-1163">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-1163">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="543f3-1164">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串>](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="543f3-1164">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="543f3-1165">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-1165">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="543f3-1166">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1166">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="543f3-1167">設定 timeout 和 keep-alive 行為的其他選項可用於`HubConnection`物件本身：</span><span class="sxs-lookup"><span data-stu-id="543f3-1167">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="543f3-1168">.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-1168">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="543f3-1169">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1169">Option</span></span> | <span data-ttu-id="543f3-1170">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1170">Default value</span></span> | <span data-ttu-id="543f3-1171">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-1171">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="543f3-1172">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-1172">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-1173">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-1173">Timeout for server activity.</span></span> <span data-ttu-id="543f3-1174">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `Closed`並觸發`onclose`事件（在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-1174">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="543f3-1175">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-1175">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-1176">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-1176">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="543f3-1177">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-1177">15 seconds</span></span> | <span data-ttu-id="543f3-1178">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-1178">Timeout for initial server handshake.</span></span> <span data-ttu-id="543f3-1179">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`Closed`事件（`onclose`在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="543f3-1179">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="543f3-1180">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1180">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-1181">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-1181">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="543f3-1182">在 .NET 用戶端中，timeout 值會指定`TimeSpan`為值。</span><span class="sxs-lookup"><span data-stu-id="543f3-1182">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="543f3-1183">JavaScript</span><span class="sxs-lookup"><span data-stu-id="543f3-1183">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="543f3-1184">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1184">Option</span></span> | <span data-ttu-id="543f3-1185">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1185">Default value</span></span> | <span data-ttu-id="543f3-1186">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-1186">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="543f3-1187">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-1187">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-1188">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-1188">Timeout for server activity.</span></span> <span data-ttu-id="543f3-1189">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onclose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-1189">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="543f3-1190">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-1190">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-1191">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-1191">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="java"></a>[<span data-ttu-id="543f3-1192">Java</span><span class="sxs-lookup"><span data-stu-id="543f3-1192">Java</span></span>](#tab/java)

| <span data-ttu-id="543f3-1193">選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1193">Option</span></span> | <span data-ttu-id="543f3-1194">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1194">Default value</span></span> | <span data-ttu-id="543f3-1195">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-1195">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="543f3-1196">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="543f3-1196">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="543f3-1197">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="543f3-1197">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="543f3-1198">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-1198">Timeout for server activity.</span></span> <span data-ttu-id="543f3-1199">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onClose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-1199">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="543f3-1200">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="543f3-1200">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="543f3-1201">建議的值是至少為伺服器`KeepAliveInterval`值的兩個數字，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="543f3-1201">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="543f3-1202">15 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-1202">15 seconds</span></span> | <span data-ttu-id="543f3-1203">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="543f3-1203">Timeout for initial server handshake.</span></span> <span data-ttu-id="543f3-1204">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="543f3-1204">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="543f3-1205">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1205">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="543f3-1206">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="543f3-1206">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="543f3-1207">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1207">Configure additional options</span></span>

<span data-ttu-id="543f3-1208">您`WithUrl`可以在上`withUrl` `HubConnectionBuilder`的（JavaScript）方法中，或在 JAVA 用戶端`HttpHubConnectionBuilder`中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-1208">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="543f3-1209">.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-1209">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="543f3-1210">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1210">.NET Option</span></span> |  <span data-ttu-id="543f3-1211">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1211">Default value</span></span> | <span data-ttu-id="543f3-1212">說明</span><span class="sxs-lookup"><span data-stu-id="543f3-1212">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="543f3-1213">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-1213">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="543f3-1214">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-1214">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-1215">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-1215">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-1216">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1216">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="543f3-1217">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-1217">Empty</span></span> | <span data-ttu-id="543f3-1218">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="543f3-1218">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="543f3-1219">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-1219">Empty</span></span> | <span data-ttu-id="543f3-1220">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="543f3-1220">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="543f3-1221">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-1221">Empty</span></span> | <span data-ttu-id="543f3-1222">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-1222">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="543f3-1223">5 秒</span><span class="sxs-lookup"><span data-stu-id="543f3-1223">5 seconds</span></span> | <span data-ttu-id="543f3-1224">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="543f3-1224">WebSockets only.</span></span> <span data-ttu-id="543f3-1225">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="543f3-1225">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="543f3-1226">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="543f3-1226">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="543f3-1227">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-1227">Empty</span></span> | <span data-ttu-id="543f3-1228">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="543f3-1228">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="543f3-1229">委派，可以用來設定或取代用來傳送`HttpMessageHandler` HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="543f3-1229">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="543f3-1230">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="543f3-1230">Not used for WebSocket connections.</span></span> <span data-ttu-id="543f3-1231">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="543f3-1231">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="543f3-1232">請修改該預設值的設定並加以傳回，或傳回新`HttpMessageHandler`的實例。</span><span class="sxs-lookup"><span data-stu-id="543f3-1232">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="543f3-1233">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="543f3-1233">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="543f3-1234">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="543f3-1234">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="543f3-1235">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="543f3-1235">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="543f3-1236">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="543f3-1236">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="543f3-1237">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="543f3-1237">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="543f3-1238">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="543f3-1238">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="543f3-1239">JavaScript</span><span class="sxs-lookup"><span data-stu-id="543f3-1239">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="543f3-1240">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1240">JavaScript Option</span></span> | <span data-ttu-id="543f3-1241">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1241">Default Value</span></span> | <span data-ttu-id="543f3-1242">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-1242">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="543f3-1243">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-1243">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="543f3-1244">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-1244">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-1245">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-1245">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-1246">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1246">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="543f3-1247">Java</span><span class="sxs-lookup"><span data-stu-id="543f3-1247">Java</span></span>](#tab/java)

| <span data-ttu-id="543f3-1248">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="543f3-1248">Java Option</span></span> | <span data-ttu-id="543f3-1249">預設值</span><span class="sxs-lookup"><span data-stu-id="543f3-1249">Default Value</span></span> | <span data-ttu-id="543f3-1250">描述</span><span class="sxs-lookup"><span data-stu-id="543f3-1250">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="543f3-1251">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="543f3-1251">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="543f3-1252">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="543f3-1252">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="543f3-1253">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="543f3-1253">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="543f3-1254">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="543f3-1254">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="543f3-1255">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="543f3-1255">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="543f3-1256">空白</span><span class="sxs-lookup"><span data-stu-id="543f3-1256">Empty</span></span> | <span data-ttu-id="543f3-1257">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="543f3-1257">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="543f3-1258">在 .NET 用戶端中，這些選項可以由提供給`WithUrl`的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="543f3-1258">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="543f3-1259">在 JavaScript 用戶端中，可以在提供給`withUrl`的 javascript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="543f3-1259">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="543f3-1260">在 JAVA 用戶端中，您可以使用所`HttpHubConnectionBuilder`傳回之上的方法來設定這些選項。`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="543f3-1260">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="543f3-1261">其他資源</span><span class="sxs-lookup"><span data-stu-id="543f3-1261">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
