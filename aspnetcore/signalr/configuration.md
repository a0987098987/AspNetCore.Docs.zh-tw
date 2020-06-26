---
title: ASP.NET Core SignalR 設定
author: bradygaster
description: 瞭解如何設定 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/12/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/configuration
ms.openlocfilehash: c711c2163908e3fdd20e3bb497f333ebd495d921
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406832"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="87bb6-103">ASP.NET Core SignalR 設定</span><span class="sxs-lookup"><span data-stu-id="87bb6-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-5.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="87bb6-104">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="87bb6-105">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="87bb6-106">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="87bb6-107">您可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，在伺服器上設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="87bb6-108">`AddJsonProtocol`可以在中的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="87bb6-109">`AddJsonProtocol`方法會接受可接收物件的委派 `options` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="87bb6-110">該物件上的[PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是 `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> 物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="87bb6-111">如需詳細資訊，請參閱[檔上的System.Text.Js](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="87bb6-112">例如，若要設定序列化程式不要變更屬性名稱的大小寫，而不是預設的 "camelCase" 名稱，請在中使用下列程式碼 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="87bb6-113">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="87bb6-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="87bb6-114">`Microsoft.Extensions.DependencyInjection`必須匯入命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="87bb6-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="87bb6-115">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="87bb6-116">切換至 Newtonsoft.Js開啟</span><span class="sxs-lookup"><span data-stu-id="87bb6-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="87bb6-117">如果您需要 `Newtonsoft.Json` 中不支援的功能 `System.Text.Json` ，請參閱[切換至上的 Newtonsoft.Js](xref:migration/22-to-30#switch-to-newtonsoftjson)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="87bb6-118">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-118">MessagePack serialization options</span></span>

<span data-ttu-id="87bb6-119">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="87bb6-120">如需詳細資訊，請參閱[中 SignalR 的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="87bb6-121">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="87bb6-122">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-122">Configure server options</span></span>

<span data-ttu-id="87bb6-123">下表說明設定中樞的選項 SignalR ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="87bb6-124">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-124">Option</span></span> | <span data-ttu-id="87bb6-125">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-125">Default Value</span></span> | <span data-ttu-id="87bb6-126">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="87bb6-127">30 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-127">30 seconds</span></span> | <span data-ttu-id="87bb6-128">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="87bb6-129">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="87bb6-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="87bb6-130">建議的值為值的雙精度浮點數 `KeepAliveInterval` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="87bb6-131">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-131">15 seconds</span></span> | <span data-ttu-id="87bb6-132">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="87bb6-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="87bb6-133">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-134">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="87bb6-135">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-135">15 seconds</span></span> | <span data-ttu-id="87bb6-136">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="87bb6-137">在變更時 `KeepAliveInterval` ，請變更 `ServerTimeout` / `serverTimeoutInMilliseconds` 用戶端上的設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="87bb6-138">建議的 `ServerTimeout` / `serverTimeoutInMilliseconds` 值為值的雙精度浮點數 `KeepAliveInterval` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="87bb6-139">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="87bb6-139">All installed protocols</span></span> | <span data-ttu-id="87bb6-140">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-140">Protocols supported by this hub.</span></span> <span data-ttu-id="87bb6-141">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="87bb6-142">如果為 `true` ，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="87bb6-143">預設值為 `false` ，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="87bb6-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="87bb6-144">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="87bb6-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="87bb6-145">若達到此限制，則在伺服器處理資料流程專案之前，會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="87bb6-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="87bb6-146">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-146">32 KB</span></span> | <span data-ttu-id="87bb6-147">單一傳入中樞訊息的大小上限。</span><span class="sxs-lookup"><span data-stu-id="87bb6-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="87bb6-148">您可以為所有中樞設定選項，方法是提供選項委派給 `AddSignalR` 中的呼叫 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="87bb6-149">單一中樞的選項會覆寫中提供的全域選項 `AddSignalR` ，並可使用下列方式進行設定 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*> ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<ChatHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="87bb6-150">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="87bb6-151">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="87bb6-152">這些選項是藉由將委派傳遞至中的[MapHub \<T> ](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub)來設定 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chathub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```

<span data-ttu-id="87bb6-153">下表描述設定 ASP.NET Core SignalR 的 ADVANCED HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="87bb6-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="87bb6-154">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-154">Option</span></span> | <span data-ttu-id="87bb6-155">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-155">Default Value</span></span> | <span data-ttu-id="87bb6-156">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="87bb6-157">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-157">32 KB</span></span> | <span data-ttu-id="87bb6-158">在套用背壓之前，伺服器會緩衝的用戶端接收的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="87bb6-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="87bb6-159">增加此值可讓伺服器更快速地接收較大的訊息，而不需套用背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="87bb6-160">從套用至中樞類別的屬性自動收集的資料 `Authorize` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="87bb6-161">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="87bb6-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="87bb6-162">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-162">32 KB</span></span> | <span data-ttu-id="87bb6-163">在觀察到背壓之前，伺服器會緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="87bb6-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="87bb6-164">增加此值可讓伺服器更快速地緩衝較大的訊息，而不需等待背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="87bb6-165">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="87bb6-165">All Transports are enabled.</span></span> | <span data-ttu-id="87bb6-166">值的位旗標列舉 `HttpTransportType` ，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="87bb6-167">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="87bb6-167">See below.</span></span> | <span data-ttu-id="87bb6-168">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="87bb6-169">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="87bb6-169">See below.</span></span> | <span data-ttu-id="87bb6-170">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-170">Additional options specific to the WebSockets transport.</span></span> |
| `MinimumProtocolVersion` | <span data-ttu-id="87bb6-171">0</span><span class="sxs-lookup"><span data-stu-id="87bb6-171">0</span></span> | <span data-ttu-id="87bb6-172">指定 negotiate 通訊協定的最低版本。</span><span class="sxs-lookup"><span data-stu-id="87bb6-172">Specify the minimum version of the negotiate protocol.</span></span> <span data-ttu-id="87bb6-173">這是用來將用戶端限制為較新的版本。</span><span class="sxs-lookup"><span data-stu-id="87bb6-173">This is used to limit clients to newer versions.</span></span> |

<span data-ttu-id="87bb6-174">長輪詢傳輸具有其他選項，可以使用屬性來設定 `LongPolling` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-174">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="87bb6-175">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-175">Option</span></span> | <span data-ttu-id="87bb6-176">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-176">Default Value</span></span> | <span data-ttu-id="87bb6-177">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-177">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="87bb6-178">90秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-178">90 seconds</span></span> | <span data-ttu-id="87bb6-179">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-179">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="87bb6-180">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="87bb6-180">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="87bb6-181">WebSocket 傳輸具有可使用屬性來設定的其他選項 `WebSockets` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-181">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="87bb6-182">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-182">Option</span></span> | <span data-ttu-id="87bb6-183">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-183">Default Value</span></span> | <span data-ttu-id="87bb6-184">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-184">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="87bb6-185">5 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-185">5 seconds</span></span> | <span data-ttu-id="87bb6-186">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="87bb6-186">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="87bb6-187">可以用來將 `Sec-WebSocket-Protocol` 標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="87bb6-187">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="87bb6-188">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-188">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="87bb6-189">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-189">Configure client options</span></span>

<span data-ttu-id="87bb6-190">您可以在類型上設定用戶端選項 `HubConnectionBuilder` （可在 .net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-190">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="87bb6-191">它也可在 JAVA 用戶端中使用，但子 `HttpHubConnectionBuilder` 類別則包含產生器設定選項，以及 `HubConnection` 本身。</span><span class="sxs-lookup"><span data-stu-id="87bb6-191">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="87bb6-192">設定記錄</span><span class="sxs-lookup"><span data-stu-id="87bb6-192">Configure logging</span></span>

<span data-ttu-id="87bb6-193">使用方法，在 .NET 用戶端中設定記錄 `ConfigureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-193">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="87bb6-194">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="87bb6-194">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="87bb6-195">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-195">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="87bb6-196">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-196">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="87bb6-197">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="87bb6-197">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="87bb6-198">例如，若要啟用主控台記錄，請安裝 `Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-198">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="87bb6-199">呼叫 `AddConsole` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="87bb6-199">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="87bb6-200">在 JavaScript 用戶端中，有類似的 `configureLogging` 方法存在。</span><span class="sxs-lookup"><span data-stu-id="87bb6-200">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="87bb6-201">提供 `LogLevel` 值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-201">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="87bb6-202">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="87bb6-202">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="87bb6-203">`LogLevel`您也可以提供 `string` 代表記錄層級名稱的值，而不是值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-203">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="87bb6-204">SignalR在您無法存取常數的環境中設定記錄功能時，這會很有用 `LogLevel` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-204">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="87bb6-205">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-205">The following table lists the available log levels.</span></span> <span data-ttu-id="87bb6-206">您所提供的值， `configureLogging` 用來設定將記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-206">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="87bb6-207">記錄在此層級的訊息，**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="87bb6-207">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="87bb6-208">String</span><span class="sxs-lookup"><span data-stu-id="87bb6-208">String</span></span>                      | <span data-ttu-id="87bb6-209">LogLevel</span><span class="sxs-lookup"><span data-stu-id="87bb6-209">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="87bb6-210">`info` **或** `information`</span><span class="sxs-lookup"><span data-stu-id="87bb6-210">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="87bb6-211">`warn` **或** `warning`</span><span class="sxs-lookup"><span data-stu-id="87bb6-211">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="87bb6-212">若要完全停用記錄，請 `signalR.LogLevel.None` 在方法中指定 `configureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-212">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="87bb6-213">如需記錄的詳細資訊，請參閱[ SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-213">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="87bb6-214">SignalRJAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="87bb6-214">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="87bb6-215">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="87bb6-215">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="87bb6-216">下列程式碼片段顯示如何搭配使用 `java.util.logging` 與 SignalR JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-216">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="87bb6-217">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="87bb6-217">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="87bb6-218">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="87bb6-218">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="87bb6-219">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="87bb6-219">Configure allowed transports</span></span>

<span data-ttu-id="87bb6-220">使用的傳輸 SignalR 可以在呼叫中設定 `WithUrl` （ `withUrl` 以 JavaScript）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-220">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="87bb6-221">值的位 OR `HttpTransportType` 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-221">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="87bb6-222">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-222">All transports are enabled by default.</span></span>

<span data-ttu-id="87bb6-223">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="87bb6-223">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="87bb6-224">在 JavaScript 用戶端中，會透過 `transport` 在提供給的選項物件上設定欄位，來設定傳輸 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-224">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="87bb6-225">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-225">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="87bb6-226">在 JAVA 用戶端中，會使用上的方法來選取傳輸 `withTransport` `HttpHubConnectionBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-226">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="87bb6-227">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-227">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="87bb6-228">SignalRJAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="87bb6-228">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="87bb6-229">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="87bb6-229">Configure bearer authentication</span></span>

<span data-ttu-id="87bb6-230">若要提供驗證資料和 SignalR 要求，請使用 `AccessTokenProvider` 選項（ `accessTokenFactory` 在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-230">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="87bb6-231">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用類型為 `Authorization` 的標頭）傳入 `Bearer` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-231">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="87bb6-232">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-232">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="87bb6-233">在這些情況下，存取權杖會以查詢字串值的形式提供 `access_token` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-233">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="87bb6-234">在 .NET 用戶端中， `AccessTokenProvider` 可以使用中的選項委派來指定選項 `WithUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-234">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="87bb6-235">在 JavaScript 用戶端中，存取權杖是藉由 `accessTokenFactory` 在的選項物件上設定欄位來設定 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-235">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

<span data-ttu-id="87bb6-236">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-236">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="87bb6-237">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[單一 \<String> ](https://reactivex.io/documentation/single.html) [RxJAVA](https://github.com/ReactiveX/RxJava) 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-237">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="87bb6-238">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-238">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="87bb6-239">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-239">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="87bb6-240">設定 timeout 和 keep-alive 行為的其他選項可用於 `HubConnection` 物件本身：</span><span class="sxs-lookup"><span data-stu-id="87bb6-240">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="87bb6-241">.NET</span><span class="sxs-lookup"><span data-stu-id="87bb6-241">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="87bb6-242">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-242">Option</span></span> | <span data-ttu-id="87bb6-243">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-243">Default value</span></span> | <span data-ttu-id="87bb6-244">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-244">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="87bb6-245">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-245">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-246">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-246">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-247">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（ `onclose` 在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-247">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="87bb6-248">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-248">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-249">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-249">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="87bb6-250">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-250">15 seconds</span></span> | <span data-ttu-id="87bb6-251">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-251">Timeout for initial server handshake.</span></span> <span data-ttu-id="87bb6-252">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（ `onclose` 在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-252">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="87bb6-253">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-253">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-254">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-254">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="87bb6-255">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-255">15 seconds</span></span> | <span data-ttu-id="87bb6-256">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-256">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-257">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-257">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-258">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-258">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="87bb6-259">在 .NET 用戶端中，timeout 值會指定為 `TimeSpan` 值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-259">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="87bb6-260">JavaScript</span><span class="sxs-lookup"><span data-stu-id="87bb6-260">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="87bb6-261">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-261">Option</span></span> | <span data-ttu-id="87bb6-262">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-262">Default value</span></span> | <span data-ttu-id="87bb6-263">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-263">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="87bb6-264">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-264">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-265">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-265">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-266">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-266">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="87bb6-267">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-267">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-268">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-268">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="87bb6-269">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-269">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-270">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-270">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-271">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-271">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-272">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-272">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="87bb6-273">Java</span><span class="sxs-lookup"><span data-stu-id="87bb6-273">Java</span></span>](#tab/java)

| <span data-ttu-id="87bb6-274">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-274">Option</span></span> | <span data-ttu-id="87bb6-275">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-275">Default value</span></span> | <span data-ttu-id="87bb6-276">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-276">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="87bb6-277">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="87bb6-277">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="87bb6-278">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-278">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-279">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-279">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-280">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-280">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="87bb6-281">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-281">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-282">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-282">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="87bb6-283">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-283">15 seconds</span></span> | <span data-ttu-id="87bb6-284">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-284">Timeout for initial server handshake.</span></span> <span data-ttu-id="87bb6-285">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-285">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="87bb6-286">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-286">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-287">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-287">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="87bb6-288">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="87bb6-288">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="87bb6-289">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-289">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-290">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-290">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-291">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-291">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-292">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-292">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="87bb6-293">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-293">Configure additional options</span></span>

<span data-ttu-id="87bb6-294">您可以在 `WithUrl` 上的（ `withUrl` JavaScript）方法中，或在 `HubConnectionBuilder` `HttpHubConnectionBuilder` JAVA 用戶端中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="87bb6-294">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="87bb6-295">.NET</span><span class="sxs-lookup"><span data-stu-id="87bb6-295">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="87bb6-296">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-296">.NET Option</span></span> |  <span data-ttu-id="87bb6-297">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-297">Default value</span></span> | <span data-ttu-id="87bb6-298">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-298">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="87bb6-299">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-299">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="87bb6-300">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-300">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-301">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-301">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-302">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-302">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="87bb6-303">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-303">Empty</span></span> | <span data-ttu-id="87bb6-304">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="87bb6-304">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="87bb6-305">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-305">Empty</span></span> | <span data-ttu-id="87bb6-306">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="87bb6-306">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="87bb6-307">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-307">Empty</span></span> | <span data-ttu-id="87bb6-308">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-308">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="87bb6-309">5 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-309">5 seconds</span></span> | <span data-ttu-id="87bb6-310">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="87bb6-310">WebSockets only.</span></span> <span data-ttu-id="87bb6-311">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-311">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="87bb6-312">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-312">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="87bb6-313">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-313">Empty</span></span> | <span data-ttu-id="87bb6-314">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="87bb6-314">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="87bb6-315">委派，可以用來設定或取代 `HttpMessageHandler` 用來傳送 HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="87bb6-315">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="87bb6-316">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="87bb6-316">Not used for WebSocket connections.</span></span> <span data-ttu-id="87bb6-317">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="87bb6-317">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="87bb6-318">請修改該預設值的設定並加以傳回，或傳回新的 `HttpMessageHandler` 實例。</span><span class="sxs-lookup"><span data-stu-id="87bb6-318">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="87bb6-319">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="87bb6-319">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="87bb6-320">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="87bb6-320">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="87bb6-321">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-321">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="87bb6-322">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-322">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="87bb6-323">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="87bb6-323">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="87bb6-324">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="87bb6-324">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="87bb6-325">JavaScript</span><span class="sxs-lookup"><span data-stu-id="87bb6-325">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="87bb6-326">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-326">JavaScript Option</span></span> | <span data-ttu-id="87bb6-327">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-327">Default Value</span></span> | <span data-ttu-id="87bb6-328">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-328">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="87bb6-329">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-329">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `headers` | `null` | <span data-ttu-id="87bb6-330">每個 HTTP 要求所傳送之標頭的字典。</span><span class="sxs-lookup"><span data-stu-id="87bb6-330">Dictionary of headers sent with every HTTP request.</span></span> <span data-ttu-id="87bb6-331">在瀏覽器中傳送標頭不適用於 Websocket 或 <xref:Microsoft.AspNetCore.Http.Connections.HttpTransportType.ServerSentEvents> 資料流程。</span><span class="sxs-lookup"><span data-stu-id="87bb6-331">Sending headers in the browser doesn't work for WebSockets or the <xref:Microsoft.AspNetCore.Http.Connections.HttpTransportType.ServerSentEvents> stream.</span></span> |
| `logMessageContent` | `null` | <span data-ttu-id="87bb6-332">將設定為 `true` ，以記錄用戶端所傳送和接收之訊息的位元組/字元。</span><span class="sxs-lookup"><span data-stu-id="87bb6-332">Set to `true` to log the bytes/chars of messages sent and received by the client.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="87bb6-333">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-333">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-334">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-334">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-335">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-335">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `withCredentials` | `true` | <span data-ttu-id="87bb6-336">指定是否要使用 CORS 要求來傳送認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-336">Specifies whether credentials will be sent with the CORS request.</span></span> <span data-ttu-id="87bb6-337">Azure App Service 使用適用于粘滯會話的 cookie，而且需要啟用此選項才能正常運作。</span><span class="sxs-lookup"><span data-stu-id="87bb6-337">Azure App Service uses cookies for sticky sessions and needs this option enabled to work correctly.</span></span> <span data-ttu-id="87bb6-338">如需有關 CORS 與的詳細資訊 SignalR ，請參閱 <xref:signalr/security#cross-origin-resource-sharing> 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-338">For more info on CORS with SignalR, see <xref:signalr/security#cross-origin-resource-sharing>.</span></span> |

# <a name="java"></a>[<span data-ttu-id="87bb6-339">Java</span><span class="sxs-lookup"><span data-stu-id="87bb6-339">Java</span></span>](#tab/java)

| <span data-ttu-id="87bb6-340">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-340">Java Option</span></span> | <span data-ttu-id="87bb6-341">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-341">Default Value</span></span> | <span data-ttu-id="87bb6-342">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-342">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="87bb6-343">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-343">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="87bb6-344">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-344">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-345">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-345">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-346">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-346">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="87bb6-347">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="87bb6-347">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="87bb6-348">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-348">Empty</span></span> | <span data-ttu-id="87bb6-349">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="87bb6-349">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="87bb6-350">在 .NET 用戶端中，這些選項可以由提供給的選項委派進行修改 `WithUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-350">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="87bb6-351">在 JavaScript 用戶端中，可以在提供給的 JavaScript 物件中提供這些選項 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-351">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="87bb6-352">在 JAVA 用戶端中，您可以使用所傳回之上的方法來設定這些選項。 `HttpHubConnectionBuilder``HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="87bb6-352">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="87bb6-353">其他資源</span><span class="sxs-lookup"><span data-stu-id="87bb6-353">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-3.1"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="87bb6-354">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-354">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="87bb6-355">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-355">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="87bb6-356">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-356">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="87bb6-357">您可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，在伺服器上設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-357">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="87bb6-358">`AddJsonProtocol`可以在中的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-358">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="87bb6-359">`AddJsonProtocol`方法會接受可接收物件的委派 `options` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-359">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="87bb6-360">該物件上的[PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是 `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> 物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-360">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="87bb6-361">如需詳細資訊，請參閱[檔上的System.Text.Js](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-361">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="87bb6-362">例如，若要設定序列化程式不要變更屬性名稱的大小寫，而不是預設的 "camelCase" 名稱，請在中使用下列程式碼 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-362">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="87bb6-363">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="87bb6-363">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="87bb6-364">`Microsoft.Extensions.DependencyInjection`必須匯入命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="87bb6-364">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="87bb6-365">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-365">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="87bb6-366">切換至 Newtonsoft.Js開啟</span><span class="sxs-lookup"><span data-stu-id="87bb6-366">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="87bb6-367">如果您需要 `Newtonsoft.Json` 中不支援的功能 `System.Text.Json` ，請參閱[切換至上的 Newtonsoft.Js](xref:migration/22-to-30#switch-to-newtonsoftjson)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-367">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="87bb6-368">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-368">MessagePack serialization options</span></span>

<span data-ttu-id="87bb6-369">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-369">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="87bb6-370">如需詳細資訊，請參閱[中 SignalR 的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-370">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="87bb6-371">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-371">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="87bb6-372">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-372">Configure server options</span></span>

<span data-ttu-id="87bb6-373">下表說明設定中樞的選項 SignalR ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-373">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="87bb6-374">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-374">Option</span></span> | <span data-ttu-id="87bb6-375">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-375">Default Value</span></span> | <span data-ttu-id="87bb6-376">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-376">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="87bb6-377">30 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-377">30 seconds</span></span> | <span data-ttu-id="87bb6-378">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-378">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="87bb6-379">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="87bb6-379">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="87bb6-380">建議的值為值的雙精度浮點數 `KeepAliveInterval` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-380">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="87bb6-381">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-381">15 seconds</span></span> | <span data-ttu-id="87bb6-382">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="87bb6-382">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="87bb6-383">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-383">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-384">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-384">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="87bb6-385">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-385">15 seconds</span></span> | <span data-ttu-id="87bb6-386">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-386">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="87bb6-387">在變更時 `KeepAliveInterval` ，請變更 `ServerTimeout` / `serverTimeoutInMilliseconds` 用戶端上的設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-387">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="87bb6-388">建議的 `ServerTimeout` / `serverTimeoutInMilliseconds` 值為值的雙精度浮點數 `KeepAliveInterval` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-388">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="87bb6-389">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="87bb6-389">All installed protocols</span></span> | <span data-ttu-id="87bb6-390">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-390">Protocols supported by this hub.</span></span> <span data-ttu-id="87bb6-391">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-391">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="87bb6-392">如果為 `true` ，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-392">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="87bb6-393">預設值為 `false` ，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="87bb6-393">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="87bb6-394">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="87bb6-394">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="87bb6-395">若達到此限制，則在伺服器處理資料流程專案之前，會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="87bb6-395">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="87bb6-396">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-396">32 KB</span></span> | <span data-ttu-id="87bb6-397">單一傳入中樞訊息的大小上限。</span><span class="sxs-lookup"><span data-stu-id="87bb6-397">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="87bb6-398">您可以為所有中樞設定選項，方法是提供選項委派給 `AddSignalR` 中的呼叫 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-398">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="87bb6-399">單一中樞的選項會覆寫中提供的全域選項 `AddSignalR` ，並可使用下列方式進行設定 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*> ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-399">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<ChatHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="87bb6-400">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-400">Advanced HTTP configuration options</span></span>

<span data-ttu-id="87bb6-401">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-401">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="87bb6-402">這些選項是藉由將委派傳遞至中的[MapHub \<T> ](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub)來設定 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-402">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chathub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```

<span data-ttu-id="87bb6-403">下表描述設定 ASP.NET Core SignalR 的 ADVANCED HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="87bb6-403">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="87bb6-404">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-404">Option</span></span> | <span data-ttu-id="87bb6-405">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-405">Default Value</span></span> | <span data-ttu-id="87bb6-406">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-406">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="87bb6-407">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-407">32 KB</span></span> | <span data-ttu-id="87bb6-408">在套用背壓之前，伺服器會緩衝的用戶端接收的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="87bb6-408">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="87bb6-409">增加此值可讓伺服器更快速地接收較大的訊息，而不需套用背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-409">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="87bb6-410">從套用至中樞類別的屬性自動收集的資料 `Authorize` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-410">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="87bb6-411">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="87bb6-411">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="87bb6-412">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-412">32 KB</span></span> | <span data-ttu-id="87bb6-413">在觀察到背壓之前，伺服器會緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="87bb6-413">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="87bb6-414">增加此值可讓伺服器更快速地緩衝較大的訊息，而不需等待背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-414">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="87bb6-415">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="87bb6-415">All Transports are enabled.</span></span> | <span data-ttu-id="87bb6-416">值的位旗標列舉 `HttpTransportType` ，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-416">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="87bb6-417">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="87bb6-417">See below.</span></span> | <span data-ttu-id="87bb6-418">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-418">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="87bb6-419">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="87bb6-419">See below.</span></span> | <span data-ttu-id="87bb6-420">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-420">Additional options specific to the WebSockets transport.</span></span> |
| `MinimumProtocolVersion` | <span data-ttu-id="87bb6-421">0</span><span class="sxs-lookup"><span data-stu-id="87bb6-421">0</span></span> | <span data-ttu-id="87bb6-422">指定 negotiate 通訊協定的最低版本。</span><span class="sxs-lookup"><span data-stu-id="87bb6-422">Specify the minimum version of the negotiate protocol.</span></span> <span data-ttu-id="87bb6-423">這是用來將用戶端限制為較新的版本。</span><span class="sxs-lookup"><span data-stu-id="87bb6-423">This is used to limit clients to newer versions.</span></span> |

<span data-ttu-id="87bb6-424">長輪詢傳輸具有其他選項，可以使用屬性來設定 `LongPolling` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-424">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="87bb6-425">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-425">Option</span></span> | <span data-ttu-id="87bb6-426">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-426">Default Value</span></span> | <span data-ttu-id="87bb6-427">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-427">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="87bb6-428">90秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-428">90 seconds</span></span> | <span data-ttu-id="87bb6-429">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-429">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="87bb6-430">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="87bb6-430">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="87bb6-431">WebSocket 傳輸具有可使用屬性來設定的其他選項 `WebSockets` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-431">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="87bb6-432">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-432">Option</span></span> | <span data-ttu-id="87bb6-433">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-433">Default Value</span></span> | <span data-ttu-id="87bb6-434">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-434">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="87bb6-435">5 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-435">5 seconds</span></span> | <span data-ttu-id="87bb6-436">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="87bb6-436">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="87bb6-437">可以用來將 `Sec-WebSocket-Protocol` 標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="87bb6-437">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="87bb6-438">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-438">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="87bb6-439">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-439">Configure client options</span></span>

<span data-ttu-id="87bb6-440">您可以在類型上設定用戶端選項 `HubConnectionBuilder` （可在 .net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-440">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="87bb6-441">它也可在 JAVA 用戶端中使用，但子 `HttpHubConnectionBuilder` 類別則包含產生器設定選項，以及 `HubConnection` 本身。</span><span class="sxs-lookup"><span data-stu-id="87bb6-441">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="87bb6-442">設定記錄</span><span class="sxs-lookup"><span data-stu-id="87bb6-442">Configure logging</span></span>

<span data-ttu-id="87bb6-443">使用方法，在 .NET 用戶端中設定記錄 `ConfigureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-443">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="87bb6-444">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="87bb6-444">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="87bb6-445">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-445">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="87bb6-446">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-446">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="87bb6-447">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="87bb6-447">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="87bb6-448">例如，若要啟用主控台記錄，請安裝 `Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-448">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="87bb6-449">呼叫 `AddConsole` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="87bb6-449">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="87bb6-450">在 JavaScript 用戶端中，有類似的 `configureLogging` 方法存在。</span><span class="sxs-lookup"><span data-stu-id="87bb6-450">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="87bb6-451">提供 `LogLevel` 值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-451">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="87bb6-452">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="87bb6-452">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="87bb6-453">`LogLevel`您也可以提供 `string` 代表記錄層級名稱的值，而不是值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-453">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="87bb6-454">SignalR在您無法存取常數的環境中設定記錄功能時，這會很有用 `LogLevel` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-454">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="87bb6-455">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-455">The following table lists the available log levels.</span></span> <span data-ttu-id="87bb6-456">您所提供的值， `configureLogging` 用來設定將記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-456">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="87bb6-457">記錄在此層級的訊息，**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="87bb6-457">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="87bb6-458">String</span><span class="sxs-lookup"><span data-stu-id="87bb6-458">String</span></span>                      | <span data-ttu-id="87bb6-459">LogLevel</span><span class="sxs-lookup"><span data-stu-id="87bb6-459">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="87bb6-460">`info` **或** `information`</span><span class="sxs-lookup"><span data-stu-id="87bb6-460">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="87bb6-461">`warn` **或** `warning`</span><span class="sxs-lookup"><span data-stu-id="87bb6-461">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="87bb6-462">若要完全停用記錄，請 `signalR.LogLevel.None` 在方法中指定 `configureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-462">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="87bb6-463">如需記錄的詳細資訊，請參閱[ SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-463">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="87bb6-464">SignalRJAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="87bb6-464">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="87bb6-465">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="87bb6-465">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="87bb6-466">下列程式碼片段顯示如何搭配使用 `java.util.logging` 與 SignalR JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-466">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="87bb6-467">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="87bb6-467">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="87bb6-468">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="87bb6-468">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="87bb6-469">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="87bb6-469">Configure allowed transports</span></span>

<span data-ttu-id="87bb6-470">使用的傳輸 SignalR 可以在呼叫中設定 `WithUrl` （ `withUrl` 以 JavaScript）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-470">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="87bb6-471">值的位 OR `HttpTransportType` 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-471">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="87bb6-472">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-472">All transports are enabled by default.</span></span>

<span data-ttu-id="87bb6-473">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="87bb6-473">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="87bb6-474">在 JavaScript 用戶端中，會透過 `transport` 在提供給的選項物件上設定欄位，來設定傳輸 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-474">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="87bb6-475">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-475">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="87bb6-476">在 JAVA 用戶端中，會使用上的方法來選取傳輸 `withTransport` `HttpHubConnectionBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-476">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="87bb6-477">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-477">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="87bb6-478">SignalRJAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="87bb6-478">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="87bb6-479">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="87bb6-479">Configure bearer authentication</span></span>

<span data-ttu-id="87bb6-480">若要提供驗證資料和 SignalR 要求，請使用 `AccessTokenProvider` 選項（ `accessTokenFactory` 在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-480">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="87bb6-481">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用類型為 `Authorization` 的標頭）傳入 `Bearer` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-481">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="87bb6-482">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-482">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="87bb6-483">在這些情況下，存取權杖會以查詢字串值的形式提供 `access_token` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-483">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="87bb6-484">在 .NET 用戶端中， `AccessTokenProvider` 可以使用中的選項委派來指定選項 `WithUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-484">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="87bb6-485">在 JavaScript 用戶端中，存取權杖是藉由 `accessTokenFactory` 在的選項物件上設定欄位來設定 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-485">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

<span data-ttu-id="87bb6-486">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-486">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="87bb6-487">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[單一 \<String> ](https://reactivex.io/documentation/single.html) [RxJAVA](https://github.com/ReactiveX/RxJava) 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-487">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="87bb6-488">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-488">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="87bb6-489">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-489">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="87bb6-490">設定 timeout 和 keep-alive 行為的其他選項可用於 `HubConnection` 物件本身：</span><span class="sxs-lookup"><span data-stu-id="87bb6-490">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="87bb6-491">.NET</span><span class="sxs-lookup"><span data-stu-id="87bb6-491">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="87bb6-492">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-492">Option</span></span> | <span data-ttu-id="87bb6-493">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-493">Default value</span></span> | <span data-ttu-id="87bb6-494">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-494">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="87bb6-495">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-495">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-496">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-496">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-497">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（ `onclose` 在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-497">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="87bb6-498">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-498">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-499">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-499">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="87bb6-500">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-500">15 seconds</span></span> | <span data-ttu-id="87bb6-501">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-501">Timeout for initial server handshake.</span></span> <span data-ttu-id="87bb6-502">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（ `onclose` 在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-502">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="87bb6-503">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-503">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-504">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-504">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="87bb6-505">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-505">15 seconds</span></span> | <span data-ttu-id="87bb6-506">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-506">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-507">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-507">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-508">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-508">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="87bb6-509">在 .NET 用戶端中，timeout 值會指定為 `TimeSpan` 值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-509">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="87bb6-510">JavaScript</span><span class="sxs-lookup"><span data-stu-id="87bb6-510">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="87bb6-511">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-511">Option</span></span> | <span data-ttu-id="87bb6-512">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-512">Default value</span></span> | <span data-ttu-id="87bb6-513">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-513">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="87bb6-514">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-514">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-515">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-515">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-516">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-516">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="87bb6-517">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-517">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-518">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-518">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="87bb6-519">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-519">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-520">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-520">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-521">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-521">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-522">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-522">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="87bb6-523">Java</span><span class="sxs-lookup"><span data-stu-id="87bb6-523">Java</span></span>](#tab/java)

| <span data-ttu-id="87bb6-524">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-524">Option</span></span> | <span data-ttu-id="87bb6-525">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-525">Default value</span></span> | <span data-ttu-id="87bb6-526">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-526">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="87bb6-527">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="87bb6-527">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="87bb6-528">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-528">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-529">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-529">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-530">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-530">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="87bb6-531">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-531">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-532">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-532">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="87bb6-533">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-533">15 seconds</span></span> | <span data-ttu-id="87bb6-534">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-534">Timeout for initial server handshake.</span></span> <span data-ttu-id="87bb6-535">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-535">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="87bb6-536">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-536">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-537">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-537">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="87bb6-538">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="87bb6-538">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="87bb6-539">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-539">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-540">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-540">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-541">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-541">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-542">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-542">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="87bb6-543">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-543">Configure additional options</span></span>

<span data-ttu-id="87bb6-544">您可以在 `WithUrl` 上的（ `withUrl` JavaScript）方法中，或在 `HubConnectionBuilder` `HttpHubConnectionBuilder` JAVA 用戶端中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="87bb6-544">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="87bb6-545">.NET</span><span class="sxs-lookup"><span data-stu-id="87bb6-545">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="87bb6-546">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-546">.NET Option</span></span> |  <span data-ttu-id="87bb6-547">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-547">Default value</span></span> | <span data-ttu-id="87bb6-548">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-548">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="87bb6-549">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-549">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="87bb6-550">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-550">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-551">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-551">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-552">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-552">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="87bb6-553">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-553">Empty</span></span> | <span data-ttu-id="87bb6-554">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="87bb6-554">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="87bb6-555">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-555">Empty</span></span> | <span data-ttu-id="87bb6-556">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="87bb6-556">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="87bb6-557">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-557">Empty</span></span> | <span data-ttu-id="87bb6-558">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-558">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="87bb6-559">5 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-559">5 seconds</span></span> | <span data-ttu-id="87bb6-560">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="87bb6-560">WebSockets only.</span></span> <span data-ttu-id="87bb6-561">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-561">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="87bb6-562">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-562">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="87bb6-563">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-563">Empty</span></span> | <span data-ttu-id="87bb6-564">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="87bb6-564">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="87bb6-565">委派，可以用來設定或取代 `HttpMessageHandler` 用來傳送 HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="87bb6-565">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="87bb6-566">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="87bb6-566">Not used for WebSocket connections.</span></span> <span data-ttu-id="87bb6-567">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="87bb6-567">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="87bb6-568">請修改該預設值的設定並加以傳回，或傳回新的 `HttpMessageHandler` 實例。</span><span class="sxs-lookup"><span data-stu-id="87bb6-568">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="87bb6-569">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="87bb6-569">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="87bb6-570">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="87bb6-570">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="87bb6-571">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-571">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="87bb6-572">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-572">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="87bb6-573">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="87bb6-573">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="87bb6-574">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="87bb6-574">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="87bb6-575">JavaScript</span><span class="sxs-lookup"><span data-stu-id="87bb6-575">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="87bb6-576">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-576">JavaScript Option</span></span> | <span data-ttu-id="87bb6-577">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-577">Default Value</span></span> | <span data-ttu-id="87bb6-578">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-578">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="87bb6-579">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-579">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `logMessageContent` | `null` | <span data-ttu-id="87bb6-580">將設定為 `true` ，以記錄用戶端所傳送和接收之訊息的位元組/字元。</span><span class="sxs-lookup"><span data-stu-id="87bb6-580">Set to `true` to log the bytes/chars of messages sent and received by the client.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="87bb6-581">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-581">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-582">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-582">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-583">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-583">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="87bb6-584">Java</span><span class="sxs-lookup"><span data-stu-id="87bb6-584">Java</span></span>](#tab/java)

| <span data-ttu-id="87bb6-585">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-585">Java Option</span></span> | <span data-ttu-id="87bb6-586">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-586">Default Value</span></span> | <span data-ttu-id="87bb6-587">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-587">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="87bb6-588">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-588">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="87bb6-589">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-589">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-590">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-590">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-591">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-591">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="87bb6-592">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="87bb6-592">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="87bb6-593">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-593">Empty</span></span> | <span data-ttu-id="87bb6-594">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="87bb6-594">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="87bb6-595">在 .NET 用戶端中，這些選項可以由提供給的選項委派進行修改 `WithUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-595">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="87bb6-596">在 JavaScript 用戶端中，可以在提供給的 JavaScript 物件中提供這些選項 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-596">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="87bb6-597">在 JAVA 用戶端中，您可以使用所傳回之上的方法來設定這些選項。 `HttpHubConnectionBuilder``HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="87bb6-597">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="87bb6-598">其他資源</span><span class="sxs-lookup"><span data-stu-id="87bb6-598">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="87bb6-599">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-599">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="87bb6-600">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-600">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="87bb6-601">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-601">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="87bb6-602">您可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，在伺服器上設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-602">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="87bb6-603">`AddJsonProtocol`可以在中的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-603">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="87bb6-604">`AddJsonProtocol`方法會接受可接收物件的委派 `options` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-604">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="87bb6-605">該物件上的[PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是 `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> 物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-605">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="87bb6-606">如需詳細資訊，請參閱[檔上的System.Text.Js](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-606">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="87bb6-607">例如，若要設定序列化程式不要變更屬性名稱的大小寫，而不是預設的 "camelCase" 名稱，請在中使用下列程式碼 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-607">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    });
```

<span data-ttu-id="87bb6-608">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="87bb6-608">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="87bb6-609">`Microsoft.Extensions.DependencyInjection`必須匯入命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="87bb6-609">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="87bb6-610">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-610">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="87bb6-611">切換至 Newtonsoft.Js開啟</span><span class="sxs-lookup"><span data-stu-id="87bb6-611">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="87bb6-612">如果您需要 `Newtonsoft.Json` 中不支援的功能 `System.Text.Json` ，請參閱[切換至上的 Newtonsoft.Js](xref:migration/22-to-30#switch-to-newtonsoftjson)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-612">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="87bb6-613">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-613">MessagePack serialization options</span></span>

<span data-ttu-id="87bb6-614">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-614">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="87bb6-615">如需詳細資訊，請參閱[中 SignalR 的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-615">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="87bb6-616">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-616">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="87bb6-617">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-617">Configure server options</span></span>

<span data-ttu-id="87bb6-618">下表說明設定中樞的選項 SignalR ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-618">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="87bb6-619">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-619">Option</span></span> | <span data-ttu-id="87bb6-620">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-620">Default Value</span></span> | <span data-ttu-id="87bb6-621">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-621">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="87bb6-622">30 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-622">30 seconds</span></span> | <span data-ttu-id="87bb6-623">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-623">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="87bb6-624">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="87bb6-624">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="87bb6-625">建議的值為值的雙精度浮點數 `KeepAliveInterval` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-625">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="87bb6-626">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-626">15 seconds</span></span> | <span data-ttu-id="87bb6-627">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="87bb6-627">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="87bb6-628">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-628">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-629">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-629">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="87bb6-630">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-630">15 seconds</span></span> | <span data-ttu-id="87bb6-631">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-631">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="87bb6-632">在變更時 `KeepAliveInterval` ，請變更 `ServerTimeout` / `serverTimeoutInMilliseconds` 用戶端上的設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-632">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="87bb6-633">建議的 `ServerTimeout` / `serverTimeoutInMilliseconds` 值為值的雙精度浮點數 `KeepAliveInterval` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-633">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="87bb6-634">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="87bb6-634">All installed protocols</span></span> | <span data-ttu-id="87bb6-635">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-635">Protocols supported by this hub.</span></span> <span data-ttu-id="87bb6-636">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-636">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="87bb6-637">如果為 `true` ，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-637">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="87bb6-638">預設值為 `false` ，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="87bb6-638">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="87bb6-639">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="87bb6-639">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="87bb6-640">若達到此限制，則在伺服器處理資料流程專案之前，會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="87bb6-640">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="87bb6-641">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-641">32 KB</span></span> | <span data-ttu-id="87bb6-642">單一傳入中樞訊息的大小上限。</span><span class="sxs-lookup"><span data-stu-id="87bb6-642">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="87bb6-643">您可以為所有中樞設定選項，方法是提供選項委派給 `AddSignalR` 中的呼叫 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-643">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="87bb6-644">單一中樞的選項會覆寫中提供的全域選項 `AddSignalR` ，並可使用下列方式進行設定 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*> ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-644">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<ChatHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="87bb6-645">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-645">Advanced HTTP configuration options</span></span>

<span data-ttu-id="87bb6-646">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-646">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="87bb6-647">這些選項是藉由將委派傳遞至中的[MapHub \<T> ](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub)來設定 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-647">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chathub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```

<span data-ttu-id="87bb6-648">下表描述設定 ASP.NET Core SignalR 的 ADVANCED HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="87bb6-648">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="87bb6-649">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-649">Option</span></span> | <span data-ttu-id="87bb6-650">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-650">Default Value</span></span> | <span data-ttu-id="87bb6-651">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-651">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="87bb6-652">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-652">32 KB</span></span> | <span data-ttu-id="87bb6-653">在套用背壓之前，伺服器會緩衝的用戶端接收的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="87bb6-653">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="87bb6-654">增加此值可讓伺服器更快速地接收較大的訊息，而不需套用背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-654">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="87bb6-655">從套用至中樞類別的屬性自動收集的資料 `Authorize` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-655">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="87bb6-656">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="87bb6-656">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="87bb6-657">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-657">32 KB</span></span> | <span data-ttu-id="87bb6-658">在觀察到背壓之前，伺服器會緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="87bb6-658">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="87bb6-659">增加此值可讓伺服器更快速地緩衝較大的訊息，而不需等待背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-659">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="87bb6-660">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="87bb6-660">All Transports are enabled.</span></span> | <span data-ttu-id="87bb6-661">值的位旗標列舉 `HttpTransportType` ，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-661">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="87bb6-662">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="87bb6-662">See below.</span></span> | <span data-ttu-id="87bb6-663">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-663">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="87bb6-664">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="87bb6-664">See below.</span></span> | <span data-ttu-id="87bb6-665">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-665">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="87bb6-666">長輪詢傳輸具有其他選項，可以使用屬性來設定 `LongPolling` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-666">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="87bb6-667">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-667">Option</span></span> | <span data-ttu-id="87bb6-668">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-668">Default Value</span></span> | <span data-ttu-id="87bb6-669">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-669">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="87bb6-670">90秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-670">90 seconds</span></span> | <span data-ttu-id="87bb6-671">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-671">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="87bb6-672">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="87bb6-672">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="87bb6-673">WebSocket 傳輸具有可使用屬性來設定的其他選項 `WebSockets` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-673">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="87bb6-674">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-674">Option</span></span> | <span data-ttu-id="87bb6-675">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-675">Default Value</span></span> | <span data-ttu-id="87bb6-676">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-676">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="87bb6-677">5 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-677">5 seconds</span></span> | <span data-ttu-id="87bb6-678">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="87bb6-678">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="87bb6-679">可以用來將 `Sec-WebSocket-Protocol` 標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="87bb6-679">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="87bb6-680">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-680">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="87bb6-681">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-681">Configure client options</span></span>

<span data-ttu-id="87bb6-682">您可以在類型上設定用戶端選項 `HubConnectionBuilder` （可在 .net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-682">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="87bb6-683">它也可在 JAVA 用戶端中使用，但子 `HttpHubConnectionBuilder` 類別則包含產生器設定選項，以及 `HubConnection` 本身。</span><span class="sxs-lookup"><span data-stu-id="87bb6-683">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="87bb6-684">設定記錄</span><span class="sxs-lookup"><span data-stu-id="87bb6-684">Configure logging</span></span>

<span data-ttu-id="87bb6-685">使用方法，在 .NET 用戶端中設定記錄 `ConfigureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-685">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="87bb6-686">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="87bb6-686">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="87bb6-687">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-687">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="87bb6-688">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-688">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="87bb6-689">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="87bb6-689">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="87bb6-690">例如，若要啟用主控台記錄，請安裝 `Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-690">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="87bb6-691">呼叫 `AddConsole` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="87bb6-691">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="87bb6-692">在 JavaScript 用戶端中，有類似的 `configureLogging` 方法存在。</span><span class="sxs-lookup"><span data-stu-id="87bb6-692">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="87bb6-693">提供 `LogLevel` 值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-693">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="87bb6-694">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="87bb6-694">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="87bb6-695">`LogLevel`您也可以提供 `string` 代表記錄層級名稱的值，而不是值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-695">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="87bb6-696">SignalR在您無法存取常數的環境中設定記錄功能時，這會很有用 `LogLevel` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-696">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="87bb6-697">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-697">The following table lists the available log levels.</span></span> <span data-ttu-id="87bb6-698">您所提供的值， `configureLogging` 用來設定將記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-698">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="87bb6-699">記錄在此層級的訊息，**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="87bb6-699">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="87bb6-700">String</span><span class="sxs-lookup"><span data-stu-id="87bb6-700">String</span></span>                      | <span data-ttu-id="87bb6-701">LogLevel</span><span class="sxs-lookup"><span data-stu-id="87bb6-701">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="87bb6-702">`info` **或** `information`</span><span class="sxs-lookup"><span data-stu-id="87bb6-702">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="87bb6-703">`warn` **或** `warning`</span><span class="sxs-lookup"><span data-stu-id="87bb6-703">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="87bb6-704">若要完全停用記錄，請 `signalR.LogLevel.None` 在方法中指定 `configureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-704">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="87bb6-705">如需記錄的詳細資訊，請參閱[ SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-705">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="87bb6-706">SignalRJAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="87bb6-706">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="87bb6-707">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="87bb6-707">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="87bb6-708">下列程式碼片段顯示如何搭配使用 `java.util.logging` 與 SignalR JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-708">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="87bb6-709">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="87bb6-709">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="87bb6-710">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="87bb6-710">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="87bb6-711">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="87bb6-711">Configure allowed transports</span></span>

<span data-ttu-id="87bb6-712">使用的傳輸 SignalR 可以在呼叫中設定 `WithUrl` （ `withUrl` 以 JavaScript）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-712">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="87bb6-713">值的位 OR `HttpTransportType` 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-713">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="87bb6-714">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-714">All transports are enabled by default.</span></span>

<span data-ttu-id="87bb6-715">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="87bb6-715">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="87bb6-716">在 JavaScript 用戶端中，會透過 `transport` 在提供給的選項物件上設定欄位，來設定傳輸 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-716">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="87bb6-717">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-717">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="87bb6-718">在 JAVA 用戶端中，會使用上的方法來選取傳輸 `withTransport` `HttpHubConnectionBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-718">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="87bb6-719">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-719">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="87bb6-720">SignalRJAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="87bb6-720">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="87bb6-721">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="87bb6-721">Configure bearer authentication</span></span>

<span data-ttu-id="87bb6-722">若要提供驗證資料和 SignalR 要求，請使用 `AccessTokenProvider` 選項（ `accessTokenFactory` 在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-722">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="87bb6-723">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用類型為 `Authorization` 的標頭）傳入 `Bearer` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-723">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="87bb6-724">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-724">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="87bb6-725">在這些情況下，存取權杖會以查詢字串值的形式提供 `access_token` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-725">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="87bb6-726">在 .NET 用戶端中， `AccessTokenProvider` 可以使用中的選項委派來指定選項 `WithUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-726">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="87bb6-727">在 JavaScript 用戶端中，存取權杖是藉由 `accessTokenFactory` 在的選項物件上設定欄位來設定 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-727">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

<span data-ttu-id="87bb6-728">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-728">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="87bb6-729">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[單一 \<String> ](https://reactivex.io/documentation/single.html) [RxJAVA](https://github.com/ReactiveX/RxJava) 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-729">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="87bb6-730">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-730">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="87bb6-731">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-731">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="87bb6-732">設定 timeout 和 keep-alive 行為的其他選項可用於 `HubConnection` 物件本身：</span><span class="sxs-lookup"><span data-stu-id="87bb6-732">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="87bb6-733">.NET</span><span class="sxs-lookup"><span data-stu-id="87bb6-733">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="87bb6-734">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-734">Option</span></span> | <span data-ttu-id="87bb6-735">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-735">Default value</span></span> | <span data-ttu-id="87bb6-736">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-736">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="87bb6-737">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-737">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-738">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-738">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-739">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（ `onclose` 在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-739">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="87bb6-740">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-740">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-741">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-741">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="87bb6-742">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-742">15 seconds</span></span> | <span data-ttu-id="87bb6-743">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-743">Timeout for initial server handshake.</span></span> <span data-ttu-id="87bb6-744">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（ `onclose` 在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-744">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="87bb6-745">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-745">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-746">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-746">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="87bb6-747">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-747">15 seconds</span></span> | <span data-ttu-id="87bb6-748">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-748">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-749">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-749">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-750">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-750">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="87bb6-751">在 .NET 用戶端中，timeout 值會指定為 `TimeSpan` 值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-751">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="87bb6-752">JavaScript</span><span class="sxs-lookup"><span data-stu-id="87bb6-752">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="87bb6-753">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-753">Option</span></span> | <span data-ttu-id="87bb6-754">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-754">Default value</span></span> | <span data-ttu-id="87bb6-755">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-755">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="87bb6-756">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-756">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-757">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-757">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-758">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-758">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="87bb6-759">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-759">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-760">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-760">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="87bb6-761">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-761">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-762">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-762">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-763">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-763">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-764">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-764">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="87bb6-765">Java</span><span class="sxs-lookup"><span data-stu-id="87bb6-765">Java</span></span>](#tab/java)

| <span data-ttu-id="87bb6-766">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-766">Option</span></span> | <span data-ttu-id="87bb6-767">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-767">Default value</span></span> | <span data-ttu-id="87bb6-768">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-768">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="87bb6-769">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="87bb6-769">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="87bb6-770">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-770">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-771">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-771">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-772">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-772">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="87bb6-773">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-773">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-774">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-774">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="87bb6-775">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-775">15 seconds</span></span> | <span data-ttu-id="87bb6-776">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-776">Timeout for initial server handshake.</span></span> <span data-ttu-id="87bb6-777">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-777">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="87bb6-778">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-778">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-779">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-779">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="87bb6-780">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="87bb6-780">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="87bb6-781">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-781">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-782">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-782">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-783">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-783">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-784">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-784">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="87bb6-785">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-785">Configure additional options</span></span>

<span data-ttu-id="87bb6-786">您可以在 `WithUrl` 上的（ `withUrl` JavaScript）方法中，或在 `HubConnectionBuilder` `HttpHubConnectionBuilder` JAVA 用戶端中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="87bb6-786">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="87bb6-787">.NET</span><span class="sxs-lookup"><span data-stu-id="87bb6-787">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="87bb6-788">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-788">.NET Option</span></span> |  <span data-ttu-id="87bb6-789">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-789">Default value</span></span> | <span data-ttu-id="87bb6-790">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-790">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="87bb6-791">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-791">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="87bb6-792">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-792">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-793">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-793">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-794">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-794">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="87bb6-795">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-795">Empty</span></span> | <span data-ttu-id="87bb6-796">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="87bb6-796">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="87bb6-797">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-797">Empty</span></span> | <span data-ttu-id="87bb6-798">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="87bb6-798">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="87bb6-799">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-799">Empty</span></span> | <span data-ttu-id="87bb6-800">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-800">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="87bb6-801">5 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-801">5 seconds</span></span> | <span data-ttu-id="87bb6-802">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="87bb6-802">WebSockets only.</span></span> <span data-ttu-id="87bb6-803">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-803">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="87bb6-804">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-804">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="87bb6-805">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-805">Empty</span></span> | <span data-ttu-id="87bb6-806">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="87bb6-806">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="87bb6-807">委派，可以用來設定或取代 `HttpMessageHandler` 用來傳送 HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="87bb6-807">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="87bb6-808">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="87bb6-808">Not used for WebSocket connections.</span></span> <span data-ttu-id="87bb6-809">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="87bb6-809">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="87bb6-810">請修改該預設值的設定並加以傳回，或傳回新的 `HttpMessageHandler` 實例。</span><span class="sxs-lookup"><span data-stu-id="87bb6-810">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="87bb6-811">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="87bb6-811">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="87bb6-812">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="87bb6-812">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="87bb6-813">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-813">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="87bb6-814">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-814">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="87bb6-815">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="87bb6-815">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="87bb6-816">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="87bb6-816">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="87bb6-817">JavaScript</span><span class="sxs-lookup"><span data-stu-id="87bb6-817">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="87bb6-818">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-818">JavaScript Option</span></span> | <span data-ttu-id="87bb6-819">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-819">Default Value</span></span> | <span data-ttu-id="87bb6-820">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-820">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="87bb6-821">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-821">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `logMessageContent` | `null` | <span data-ttu-id="87bb6-822">將設定為 `true` ，以記錄用戶端所傳送和接收之訊息的位元組/字元。</span><span class="sxs-lookup"><span data-stu-id="87bb6-822">Set to `true` to log the bytes/chars of messages sent and received by the client.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="87bb6-823">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-823">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-824">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-824">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-825">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-825">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="87bb6-826">Java</span><span class="sxs-lookup"><span data-stu-id="87bb6-826">Java</span></span>](#tab/java)

| <span data-ttu-id="87bb6-827">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-827">Java Option</span></span> | <span data-ttu-id="87bb6-828">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-828">Default Value</span></span> | <span data-ttu-id="87bb6-829">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-829">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="87bb6-830">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-830">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="87bb6-831">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-831">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-832">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-832">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-833">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-833">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="87bb6-834">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="87bb6-834">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="87bb6-835">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-835">Empty</span></span> | <span data-ttu-id="87bb6-836">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="87bb6-836">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="87bb6-837">在 .NET 用戶端中，這些選項可以由提供給的選項委派進行修改 `WithUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-837">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="87bb6-838">在 JavaScript 用戶端中，可以在提供給的 JavaScript 物件中提供這些選項 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-838">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="87bb6-839">在 JAVA 用戶端中，您可以使用所傳回之上的方法來設定這些選項。 `HttpHubConnectionBuilder``HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="87bb6-839">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="87bb6-840">其他資源</span><span class="sxs-lookup"><span data-stu-id="87bb6-840">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="87bb6-841">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-841">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="87bb6-842">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-842">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="87bb6-843">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-843">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="87bb6-844">您可以在伺服器上使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法來設定 JSON 序列化，這可在您的方法中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-844">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="87bb6-845">`AddJsonProtocol`方法會接受可接收物件的委派 `options` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-845">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="87bb6-846">該物件上的[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性是 JSON.NET `JsonSerializerSettings` 物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-846">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="87bb6-847">如需詳細資訊，請參閱[JSON.NET 檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-847">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="87bb6-848">例如，若要設定序列化程式使用 "PascalCase" 屬性名稱，而不是預設的 "camelCase" 名稱，請在中使用下列程式碼 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-848">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="87bb6-849">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="87bb6-849">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="87bb6-850">`Microsoft.Extensions.DependencyInjection`必須匯入命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="87bb6-850">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="87bb6-851">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-851">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="87bb6-852">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-852">MessagePack serialization options</span></span>

<span data-ttu-id="87bb6-853">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-853">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="87bb6-854">如需詳細資訊，請參閱[中 SignalR 的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-854">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="87bb6-855">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-855">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="87bb6-856">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-856">Configure server options</span></span>

<span data-ttu-id="87bb6-857">下表說明設定中樞的選項 SignalR ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-857">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="87bb6-858">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-858">Option</span></span> | <span data-ttu-id="87bb6-859">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-859">Default Value</span></span> | <span data-ttu-id="87bb6-860">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-860">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="87bb6-861">30 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-861">30 seconds</span></span> | <span data-ttu-id="87bb6-862">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-862">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="87bb6-863">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="87bb6-863">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="87bb6-864">建議的值為值的雙精度浮點數 `KeepAliveInterval` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-864">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="87bb6-865">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-865">15 seconds</span></span> | <span data-ttu-id="87bb6-866">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="87bb6-866">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="87bb6-867">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-867">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-868">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-868">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="87bb6-869">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-869">15 seconds</span></span> | <span data-ttu-id="87bb6-870">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-870">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="87bb6-871">在變更時 `KeepAliveInterval` ，請變更 `ServerTimeout` / `serverTimeoutInMilliseconds` 用戶端上的設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-871">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="87bb6-872">建議的 `ServerTimeout` / `serverTimeoutInMilliseconds` 值為值的雙精度浮點數 `KeepAliveInterval` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-872">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="87bb6-873">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="87bb6-873">All installed protocols</span></span> | <span data-ttu-id="87bb6-874">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-874">Protocols supported by this hub.</span></span> <span data-ttu-id="87bb6-875">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-875">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="87bb6-876">如果為 `true` ，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-876">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="87bb6-877">預設值為 `false` ，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="87bb6-877">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="87bb6-878">您可以為所有中樞設定選項，方法是提供選項委派給 `AddSignalR` 中的呼叫 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-878">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="87bb6-879">單一中樞的選項會覆寫中提供的全域選項 `AddSignalR` ，並可使用下列方式進行設定 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*> ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-879">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<ChatHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="87bb6-880">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-880">Advanced HTTP configuration options</span></span>

<span data-ttu-id="87bb6-881">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-881">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="87bb6-882">這些選項是藉由將委派傳遞至中的[MapHub \<T> ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)來設定 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-882">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) =>
    {
        var desiredTransports =
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<ChatHub>("/chathub", (options) =>
        {
            options.Transports = desiredTransports;
        });
    });
}
```

<span data-ttu-id="87bb6-883">下表描述設定 ASP.NET Core SignalR 的 ADVANCED HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="87bb6-883">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="87bb6-884">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-884">Option</span></span> | <span data-ttu-id="87bb6-885">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-885">Default Value</span></span> | <span data-ttu-id="87bb6-886">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-886">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="87bb6-887">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-887">32 KB</span></span> | <span data-ttu-id="87bb6-888">從用戶端接收伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="87bb6-888">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="87bb6-889">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="87bb6-889">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="87bb6-890">從套用至中樞類別的屬性自動收集的資料 `Authorize` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-890">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="87bb6-891">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="87bb6-891">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="87bb6-892">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-892">32 KB</span></span> | <span data-ttu-id="87bb6-893">由伺服器所緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="87bb6-893">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="87bb6-894">增加這個值可讓伺服器傳送較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="87bb6-894">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="87bb6-895">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="87bb6-895">All Transports are enabled.</span></span> | <span data-ttu-id="87bb6-896">值的位旗標列舉 `HttpTransportType` ，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-896">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="87bb6-897">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="87bb6-897">See below.</span></span> | <span data-ttu-id="87bb6-898">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-898">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="87bb6-899">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="87bb6-899">See below.</span></span> | <span data-ttu-id="87bb6-900">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-900">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="87bb6-901">長輪詢傳輸具有其他選項，可以使用屬性來設定 `LongPolling` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-901">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="87bb6-902">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-902">Option</span></span> | <span data-ttu-id="87bb6-903">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-903">Default Value</span></span> | <span data-ttu-id="87bb6-904">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-904">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="87bb6-905">90秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-905">90 seconds</span></span> | <span data-ttu-id="87bb6-906">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-906">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="87bb6-907">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="87bb6-907">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="87bb6-908">WebSocket 傳輸具有可使用屬性來設定的其他選項 `WebSockets` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-908">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="87bb6-909">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-909">Option</span></span> | <span data-ttu-id="87bb6-910">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-910">Default Value</span></span> | <span data-ttu-id="87bb6-911">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-911">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="87bb6-912">5 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-912">5 seconds</span></span> | <span data-ttu-id="87bb6-913">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="87bb6-913">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="87bb6-914">可以用來將 `Sec-WebSocket-Protocol` 標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="87bb6-914">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="87bb6-915">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-915">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="87bb6-916">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-916">Configure client options</span></span>

<span data-ttu-id="87bb6-917">您可以在類型上設定用戶端選項 `HubConnectionBuilder` （可在 .net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-917">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="87bb6-918">它也可在 JAVA 用戶端中使用，但子 `HttpHubConnectionBuilder` 類別則包含產生器設定選項，以及 `HubConnection` 本身。</span><span class="sxs-lookup"><span data-stu-id="87bb6-918">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="87bb6-919">設定記錄</span><span class="sxs-lookup"><span data-stu-id="87bb6-919">Configure logging</span></span>

<span data-ttu-id="87bb6-920">使用方法，在 .NET 用戶端中設定記錄 `ConfigureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-920">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="87bb6-921">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="87bb6-921">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="87bb6-922">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-922">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="87bb6-923">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-923">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="87bb6-924">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="87bb6-924">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="87bb6-925">例如，若要啟用主控台記錄，請安裝 `Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-925">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="87bb6-926">呼叫 `AddConsole` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="87bb6-926">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="87bb6-927">在 JavaScript 用戶端中，有類似的 `configureLogging` 方法存在。</span><span class="sxs-lookup"><span data-stu-id="87bb6-927">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="87bb6-928">提供 `LogLevel` 值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-928">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="87bb6-929">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="87bb6-929">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="87bb6-930">若要完全停用記錄，請 `signalR.LogLevel.None` 在方法中指定 `configureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-930">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="87bb6-931">如需記錄的詳細資訊，請參閱[ SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-931">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="87bb6-932">SignalRJAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="87bb6-932">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="87bb6-933">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="87bb6-933">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="87bb6-934">下列程式碼片段顯示如何搭配使用 `java.util.logging` 與 SignalR JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-934">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="87bb6-935">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="87bb6-935">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="87bb6-936">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="87bb6-936">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="87bb6-937">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="87bb6-937">Configure allowed transports</span></span>

<span data-ttu-id="87bb6-938">使用的傳輸 SignalR 可以在呼叫中設定 `WithUrl` （ `withUrl` 以 JavaScript）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-938">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="87bb6-939">值的位 OR `HttpTransportType` 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-939">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="87bb6-940">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-940">All transports are enabled by default.</span></span>

<span data-ttu-id="87bb6-941">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="87bb6-941">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="87bb6-942">在 JavaScript 用戶端中，會透過 `transport` 在提供給的選項物件上設定欄位，來設定傳輸 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-942">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="87bb6-943">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-943">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="87bb6-944">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="87bb6-944">Configure bearer authentication</span></span>

<span data-ttu-id="87bb6-945">若要提供驗證資料和 SignalR 要求，請使用 `AccessTokenProvider` 選項（ `accessTokenFactory` 在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-945">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="87bb6-946">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用類型為 `Authorization` 的標頭）傳入 `Bearer` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-946">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="87bb6-947">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-947">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="87bb6-948">在這些情況下，存取權杖會以查詢字串值的形式提供 `access_token` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-948">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="87bb6-949">在 .NET 用戶端中， `AccessTokenProvider` 可以使用中的選項委派來指定選項 `WithUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-949">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="87bb6-950">在 JavaScript 用戶端中，存取權杖是藉由 `accessTokenFactory` 在的選項物件上設定欄位來設定 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-950">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

<span data-ttu-id="87bb6-951">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-951">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="87bb6-952">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[單一 \<String> ](https://reactivex.io/documentation/single.html) [RxJAVA](https://github.com/ReactiveX/RxJava) 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-952">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="87bb6-953">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-953">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="87bb6-954">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-954">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="87bb6-955">設定 timeout 和 keep-alive 行為的其他選項可用於 `HubConnection` 物件本身：</span><span class="sxs-lookup"><span data-stu-id="87bb6-955">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="87bb6-956">.NET</span><span class="sxs-lookup"><span data-stu-id="87bb6-956">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="87bb6-957">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-957">Option</span></span> | <span data-ttu-id="87bb6-958">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-958">Default value</span></span> | <span data-ttu-id="87bb6-959">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-959">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="87bb6-960">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-960">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-961">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-961">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-962">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（ `onclose` 在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-962">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="87bb6-963">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-963">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-964">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-964">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="87bb6-965">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-965">15 seconds</span></span> | <span data-ttu-id="87bb6-966">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-966">Timeout for initial server handshake.</span></span> <span data-ttu-id="87bb6-967">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（ `onclose` 在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-967">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="87bb6-968">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-968">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-969">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-969">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="87bb6-970">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-970">15 seconds</span></span> | <span data-ttu-id="87bb6-971">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-971">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-972">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-972">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-973">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-973">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="87bb6-974">在 .NET 用戶端中，timeout 值會指定為 `TimeSpan` 值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-974">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="87bb6-975">JavaScript</span><span class="sxs-lookup"><span data-stu-id="87bb6-975">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="87bb6-976">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-976">Option</span></span> | <span data-ttu-id="87bb6-977">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-977">Default value</span></span> | <span data-ttu-id="87bb6-978">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-978">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="87bb6-979">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-979">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-980">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-980">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-981">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-981">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="87bb6-982">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-982">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-983">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-983">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="87bb6-984">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-984">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-985">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-985">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-986">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-986">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-987">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-987">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="87bb6-988">Java</span><span class="sxs-lookup"><span data-stu-id="87bb6-988">Java</span></span>](#tab/java)

| <span data-ttu-id="87bb6-989">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-989">Option</span></span> | <span data-ttu-id="87bb6-990">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-990">Default value</span></span> | <span data-ttu-id="87bb6-991">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-991">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="87bb6-992">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="87bb6-992">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="87bb6-993">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-993">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-994">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-994">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-995">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-995">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="87bb6-996">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-996">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-997">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-997">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="87bb6-998">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-998">15 seconds</span></span> | <span data-ttu-id="87bb6-999">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-999">Timeout for initial server handshake.</span></span> <span data-ttu-id="87bb6-1000">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1000">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="87bb6-1001">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1001">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-1002">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1002">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="87bb6-1003">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="87bb6-1003">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="87bb6-1004">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-1004">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-1005">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1005">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="87bb6-1006">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1006">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="87bb6-1007">如果用戶端未在伺服器上的集合中傳送訊息 `ClientTimeoutInterval` ，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1007">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="87bb6-1008">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1008">Configure additional options</span></span>

<span data-ttu-id="87bb6-1009">您可以在 `WithUrl` 上的（ `withUrl` JavaScript）方法中，或在 `HubConnectionBuilder` `HttpHubConnectionBuilder` JAVA 用戶端中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1009">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="87bb6-1010">.NET</span><span class="sxs-lookup"><span data-stu-id="87bb6-1010">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="87bb6-1011">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1011">.NET Option</span></span> |  <span data-ttu-id="87bb6-1012">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1012">Default value</span></span> | <span data-ttu-id="87bb6-1013">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-1013">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="87bb6-1014">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1014">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="87bb6-1015">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1015">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-1016">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1016">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-1017">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1017">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="87bb6-1018">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-1018">Empty</span></span> | <span data-ttu-id="87bb6-1019">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1019">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="87bb6-1020">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-1020">Empty</span></span> | <span data-ttu-id="87bb6-1021">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1021">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="87bb6-1022">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-1022">Empty</span></span> | <span data-ttu-id="87bb6-1023">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1023">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="87bb6-1024">5 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-1024">5 seconds</span></span> | <span data-ttu-id="87bb6-1025">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1025">WebSockets only.</span></span> <span data-ttu-id="87bb6-1026">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1026">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="87bb6-1027">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1027">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="87bb6-1028">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-1028">Empty</span></span> | <span data-ttu-id="87bb6-1029">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1029">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="87bb6-1030">委派，可以用來設定或取代 `HttpMessageHandler` 用來傳送 HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1030">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="87bb6-1031">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1031">Not used for WebSocket connections.</span></span> <span data-ttu-id="87bb6-1032">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1032">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="87bb6-1033">請修改該預設值的設定並加以傳回，或傳回新的 `HttpMessageHandler` 實例。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1033">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="87bb6-1034">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="87bb6-1034">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="87bb6-1035">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1035">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="87bb6-1036">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1036">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="87bb6-1037">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1037">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="87bb6-1038">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1038">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="87bb6-1039">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1039">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="87bb6-1040">JavaScript</span><span class="sxs-lookup"><span data-stu-id="87bb6-1040">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="87bb6-1041">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1041">JavaScript Option</span></span> | <span data-ttu-id="87bb6-1042">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1042">Default Value</span></span> | <span data-ttu-id="87bb6-1043">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-1043">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="87bb6-1044">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1044">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `logMessageContent` | `null` | <span data-ttu-id="87bb6-1045">將設定為 `true` ，以記錄用戶端所傳送和接收之訊息的位元組/字元。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1045">Set to `true` to log the bytes/chars of messages sent and received by the client.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="87bb6-1046">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1046">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-1047">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1047">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-1048">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1048">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="87bb6-1049">Java</span><span class="sxs-lookup"><span data-stu-id="87bb6-1049">Java</span></span>](#tab/java)

| <span data-ttu-id="87bb6-1050">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1050">Java Option</span></span> | <span data-ttu-id="87bb6-1051">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1051">Default Value</span></span> | <span data-ttu-id="87bb6-1052">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-1052">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="87bb6-1053">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1053">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="87bb6-1054">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1054">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-1055">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1055">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-1056">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1056">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="87bb6-1057">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="87bb6-1057">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="87bb6-1058">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-1058">Empty</span></span> | <span data-ttu-id="87bb6-1059">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1059">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="87bb6-1060">在 .NET 用戶端中，這些選項可以由提供給的選項委派進行修改 `WithUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1060">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="87bb6-1061">在 JavaScript 用戶端中，可以在提供給的 JavaScript 物件中提供這些選項 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1061">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="87bb6-1062">在 JAVA 用戶端中，您可以使用所傳回之上的方法來設定這些選項。 `HttpHubConnectionBuilder``HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="87bb6-1062">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="87bb6-1063">其他資源</span><span class="sxs-lookup"><span data-stu-id="87bb6-1063">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="87bb6-1064">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1064">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="87bb6-1065">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1065">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="87bb6-1066">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1066">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="87bb6-1067">您可以在伺服器上使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法來設定 JSON 序列化，這可在您的方法中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1067">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="87bb6-1068">`AddJsonProtocol`方法會接受可接收物件的委派 `options` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1068">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="87bb6-1069">該物件上的[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性是 JSON.NET `JsonSerializerSettings` 物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1069">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="87bb6-1070">如需詳細資訊，請參閱[JSON.NET 檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1070">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="87bb6-1071">例如，若要設定序列化程式使用 "PascalCase" 屬性名稱，而不是預設的 "camelCase" 名稱，請在中使用下列程式碼 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1071">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="87bb6-1072">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1072">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="87bb6-1073">`Microsoft.Extensions.DependencyInjection`必須匯入命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1073">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="87bb6-1074">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1074">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="87bb6-1075">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1075">MessagePack serialization options</span></span>

<span data-ttu-id="87bb6-1076">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1076">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="87bb6-1077">如需詳細資訊，請參閱[中 SignalR 的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1077">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="87bb6-1078">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1078">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="87bb6-1079">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1079">Configure server options</span></span>

<span data-ttu-id="87bb6-1080">下表說明設定中樞的選項 SignalR ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1080">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="87bb6-1081">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1081">Option</span></span> | <span data-ttu-id="87bb6-1082">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1082">Default Value</span></span> | <span data-ttu-id="87bb6-1083">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-1083">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="87bb6-1084">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-1084">15 seconds</span></span> | <span data-ttu-id="87bb6-1085">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1085">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="87bb6-1086">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1086">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-1087">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1087">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="87bb6-1088">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-1088">15 seconds</span></span> | <span data-ttu-id="87bb6-1089">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1089">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="87bb6-1090">在變更時 `KeepAliveInterval` ，請變更 `ServerTimeout` / `serverTimeoutInMilliseconds` 用戶端上的設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1090">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="87bb6-1091">建議的 `ServerTimeout` / `serverTimeoutInMilliseconds` 值為值的雙精度浮點數 `KeepAliveInterval` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1091">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="87bb6-1092">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="87bb6-1092">All installed protocols</span></span> | <span data-ttu-id="87bb6-1093">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1093">Protocols supported by this hub.</span></span> <span data-ttu-id="87bb6-1094">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1094">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="87bb6-1095">如果為 `true` ，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1095">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="87bb6-1096">預設值為 `false` ，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1096">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="87bb6-1097">您可以為所有中樞設定選項，方法是提供選項委派給 `AddSignalR` 中的呼叫 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1097">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="87bb6-1098">單一中樞的選項會覆寫中提供的全域選項 `AddSignalR` ，並可使用下列方式進行設定 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*> ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1098">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<ChatHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="87bb6-1099">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1099">Advanced HTTP configuration options</span></span>

<span data-ttu-id="87bb6-1100">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1100">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="87bb6-1101">這些選項是藉由將委派傳遞至中的[MapHub \<T> ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)來設定 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1101">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) =>
    {
        var desiredTransports =
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<ChatHub>("/chathub", (options) =>
        {
            options.Transports = desiredTransports;
        });
    });
}
```

<span data-ttu-id="87bb6-1102">下表描述設定 ASP.NET Core SignalR 的 ADVANCED HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1102">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="87bb6-1103">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1103">Option</span></span> | <span data-ttu-id="87bb6-1104">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1104">Default Value</span></span> | <span data-ttu-id="87bb6-1105">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-1105">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="87bb6-1106">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-1106">32 KB</span></span> | <span data-ttu-id="87bb6-1107">從用戶端接收伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1107">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="87bb6-1108">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1108">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="87bb6-1109">從套用至中樞類別的屬性自動收集的資料 `Authorize` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1109">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="87bb6-1110">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1110">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="87bb6-1111">32 KB</span><span class="sxs-lookup"><span data-stu-id="87bb6-1111">32 KB</span></span> | <span data-ttu-id="87bb6-1112">由伺服器所緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1112">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="87bb6-1113">增加這個值可讓伺服器傳送較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1113">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="87bb6-1114">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1114">All Transports are enabled.</span></span> | <span data-ttu-id="87bb6-1115">值的位旗標列舉 `HttpTransportType` ，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1115">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="87bb6-1116">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1116">See below.</span></span> | <span data-ttu-id="87bb6-1117">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1117">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="87bb6-1118">請參閱下列內容。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1118">See below.</span></span> | <span data-ttu-id="87bb6-1119">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1119">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="87bb6-1120">長輪詢傳輸具有其他選項，可以使用屬性來設定 `LongPolling` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1120">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="87bb6-1121">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1121">Option</span></span> | <span data-ttu-id="87bb6-1122">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1122">Default Value</span></span> | <span data-ttu-id="87bb6-1123">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-1123">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="87bb6-1124">90秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-1124">90 seconds</span></span> | <span data-ttu-id="87bb6-1125">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1125">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="87bb6-1126">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1126">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="87bb6-1127">WebSocket 傳輸具有可使用屬性來設定的其他選項 `WebSockets` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1127">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="87bb6-1128">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1128">Option</span></span> | <span data-ttu-id="87bb6-1129">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1129">Default Value</span></span> | <span data-ttu-id="87bb6-1130">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-1130">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="87bb6-1131">5 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-1131">5 seconds</span></span> | <span data-ttu-id="87bb6-1132">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1132">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="87bb6-1133">可以用來將 `Sec-WebSocket-Protocol` 標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1133">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="87bb6-1134">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1134">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="87bb6-1135">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1135">Configure client options</span></span>

<span data-ttu-id="87bb6-1136">您可以在類型上設定用戶端選項 `HubConnectionBuilder` （可在 .net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1136">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="87bb6-1137">它也可在 JAVA 用戶端中使用，但子 `HttpHubConnectionBuilder` 類別則包含產生器設定選項，以及 `HubConnection` 本身。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1137">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="87bb6-1138">設定記錄</span><span class="sxs-lookup"><span data-stu-id="87bb6-1138">Configure logging</span></span>

<span data-ttu-id="87bb6-1139">使用方法，在 .NET 用戶端中設定記錄 `ConfigureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1139">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="87bb6-1140">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1140">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="87bb6-1141">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1141">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="87bb6-1142">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1142">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="87bb6-1143">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1143">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="87bb6-1144">例如，若要啟用主控台記錄，請安裝 `Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1144">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="87bb6-1145">呼叫 `AddConsole` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1145">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="87bb6-1146">在 JavaScript 用戶端中，有類似的 `configureLogging` 方法存在。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1146">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="87bb6-1147">提供 `LogLevel` 值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1147">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="87bb6-1148">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1148">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="87bb6-1149">若要完全停用記錄，請 `signalR.LogLevel.None` 在方法中指定 `configureLogging` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1149">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="87bb6-1150">如需記錄的詳細資訊，請參閱[ SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1150">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="87bb6-1151">SignalRJAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1151">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="87bb6-1152">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1152">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="87bb6-1153">下列程式碼片段顯示如何搭配使用 `java.util.logging` 與 SignalR JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1153">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="87bb6-1154">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1154">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="87bb6-1155">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1155">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="87bb6-1156">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="87bb6-1156">Configure allowed transports</span></span>

<span data-ttu-id="87bb6-1157">使用的傳輸 SignalR 可以在呼叫中設定 `WithUrl` （ `withUrl` 以 JavaScript）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1157">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="87bb6-1158">值的位 OR `HttpTransportType` 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1158">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="87bb6-1159">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1159">All transports are enabled by default.</span></span>

<span data-ttu-id="87bb6-1160">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1160">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="87bb6-1161">在 JavaScript 用戶端中，會透過 `transport` 在提供給的選項物件上設定欄位，來設定傳輸 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1161">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="87bb6-1162">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="87bb6-1162">Configure bearer authentication</span></span>

<span data-ttu-id="87bb6-1163">若要提供驗證資料和 SignalR 要求，請使用 `AccessTokenProvider` 選項（ `accessTokenFactory` 在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1163">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="87bb6-1164">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用類型為 `Authorization` 的標頭）傳入 `Bearer` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1164">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="87bb6-1165">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1165">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="87bb6-1166">在這些情況下，存取權杖會以查詢字串值的形式提供 `access_token` 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1166">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="87bb6-1167">在 .NET 用戶端中， `AccessTokenProvider` 可以使用中的選項委派來指定選項 `WithUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1167">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="87bb6-1168">在 JavaScript 用戶端中，存取權杖是藉由 `accessTokenFactory` 在的選項物件上設定欄位來設定 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1168">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

<span data-ttu-id="87bb6-1169">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1169">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="87bb6-1170">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[單一 \<String> ](https://reactivex.io/documentation/single.html) [RxJAVA](https://github.com/ReactiveX/RxJava) 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1170">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="87bb6-1171">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1171">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="87bb6-1172">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1172">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="87bb6-1173">設定 timeout 和 keep-alive 行為的其他選項可用於 `HubConnection` 物件本身：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1173">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="87bb6-1174">.NET</span><span class="sxs-lookup"><span data-stu-id="87bb6-1174">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="87bb6-1175">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1175">Option</span></span> | <span data-ttu-id="87bb6-1176">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1176">Default value</span></span> | <span data-ttu-id="87bb6-1177">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-1177">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="87bb6-1178">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-1178">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-1179">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1179">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-1180">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（ `onclose` 在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1180">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="87bb6-1181">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1181">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-1182">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1182">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="87bb6-1183">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-1183">15 seconds</span></span> | <span data-ttu-id="87bb6-1184">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1184">Timeout for initial server handshake.</span></span> <span data-ttu-id="87bb6-1185">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（ `onclose` 在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1185">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="87bb6-1186">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1186">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-1187">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1187">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="87bb6-1188">在 .NET 用戶端中，timeout 值會指定為 `TimeSpan` 值。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1188">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="87bb6-1189">JavaScript</span><span class="sxs-lookup"><span data-stu-id="87bb6-1189">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="87bb6-1190">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1190">Option</span></span> | <span data-ttu-id="87bb6-1191">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1191">Default value</span></span> | <span data-ttu-id="87bb6-1192">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-1192">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="87bb6-1193">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-1193">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-1194">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1194">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-1195">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1195">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="87bb6-1196">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1196">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-1197">建議的值是至少兩個伺服器值的數位 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1197">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="java"></a>[<span data-ttu-id="87bb6-1198">Java</span><span class="sxs-lookup"><span data-stu-id="87bb6-1198">Java</span></span>](#tab/java)

| <span data-ttu-id="87bb6-1199">選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1199">Option</span></span> | <span data-ttu-id="87bb6-1200">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1200">Default value</span></span> | <span data-ttu-id="87bb6-1201">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-1201">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="87bb6-1202">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="87bb6-1202">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="87bb6-1203">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="87bb6-1203">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="87bb6-1204">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1204">Timeout for server activity.</span></span> <span data-ttu-id="87bb6-1205">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1205">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="87bb6-1206">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1206">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="87bb6-1207">建議的值是至少為伺服器值的兩個數字 `KeepAliveInterval` ，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1207">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="87bb6-1208">15 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-1208">15 seconds</span></span> | <span data-ttu-id="87bb6-1209">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1209">Timeout for initial server handshake.</span></span> <span data-ttu-id="87bb6-1210">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1210">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="87bb6-1211">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1211">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="87bb6-1212">如需交握程式的詳細資訊，請參閱[ SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1212">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="87bb6-1213">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1213">Configure additional options</span></span>

<span data-ttu-id="87bb6-1214">您可以在 `WithUrl` 上的（ `withUrl` JavaScript）方法中，或在 `HubConnectionBuilder` `HttpHubConnectionBuilder` JAVA 用戶端中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1214">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="87bb6-1215">.NET</span><span class="sxs-lookup"><span data-stu-id="87bb6-1215">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="87bb6-1216">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1216">.NET Option</span></span> |  <span data-ttu-id="87bb6-1217">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1217">Default value</span></span> | <span data-ttu-id="87bb6-1218">描述</span><span class="sxs-lookup"><span data-stu-id="87bb6-1218">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="87bb6-1219">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1219">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="87bb6-1220">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1220">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-1221">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1221">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-1222">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1222">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="87bb6-1223">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-1223">Empty</span></span> | <span data-ttu-id="87bb6-1224">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1224">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="87bb6-1225">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-1225">Empty</span></span> | <span data-ttu-id="87bb6-1226">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1226">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="87bb6-1227">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-1227">Empty</span></span> | <span data-ttu-id="87bb6-1228">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1228">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="87bb6-1229">5 秒</span><span class="sxs-lookup"><span data-stu-id="87bb6-1229">5 seconds</span></span> | <span data-ttu-id="87bb6-1230">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1230">WebSockets only.</span></span> <span data-ttu-id="87bb6-1231">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1231">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="87bb6-1232">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1232">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="87bb6-1233">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-1233">Empty</span></span> | <span data-ttu-id="87bb6-1234">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1234">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="87bb6-1235">委派，可以用來設定或取代 `HttpMessageHandler` 用來傳送 HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1235">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="87bb6-1236">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1236">Not used for WebSocket connections.</span></span> <span data-ttu-id="87bb6-1237">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1237">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="87bb6-1238">請修改該預設值的設定並加以傳回，或傳回新的 `HttpMessageHandler` 實例。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1238">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="87bb6-1239">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="87bb6-1239">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="87bb6-1240">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1240">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="87bb6-1241">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1241">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="87bb6-1242">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1242">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="87bb6-1243">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1243">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="87bb6-1244">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1244">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="87bb6-1245">JavaScript</span><span class="sxs-lookup"><span data-stu-id="87bb6-1245">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="87bb6-1246">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1246">JavaScript Option</span></span> | <span data-ttu-id="87bb6-1247">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1247">Default Value</span></span> | <span data-ttu-id="87bb6-1248">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-1248">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="87bb6-1249">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1249">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `logMessageContent` | `null` | <span data-ttu-id="87bb6-1250">將設定為 `true` ，以記錄用戶端所傳送和接收之訊息的位元組/字元。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1250">Set to `true` to log the bytes/chars of messages sent and received by the client.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="87bb6-1251">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1251">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-1252">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1252">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-1253">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1253">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="87bb6-1254">Java</span><span class="sxs-lookup"><span data-stu-id="87bb6-1254">Java</span></span>](#tab/java)

| <span data-ttu-id="87bb6-1255">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="87bb6-1255">Java Option</span></span> | <span data-ttu-id="87bb6-1256">預設值</span><span class="sxs-lookup"><span data-stu-id="87bb6-1256">Default Value</span></span> | <span data-ttu-id="87bb6-1257">說明</span><span class="sxs-lookup"><span data-stu-id="87bb6-1257">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="87bb6-1258">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1258">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="87bb6-1259">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1259">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="87bb6-1260">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1260">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="87bb6-1261">使用 Azure 服務時，無法啟用此設定 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1261">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="87bb6-1262">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="87bb6-1262">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="87bb6-1263">空白</span><span class="sxs-lookup"><span data-stu-id="87bb6-1263">Empty</span></span> | <span data-ttu-id="87bb6-1264">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="87bb6-1264">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="87bb6-1265">在 .NET 用戶端中，這些選項可以由提供給的選項委派進行修改 `WithUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1265">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/chathub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="87bb6-1266">在 JavaScript 用戶端中，可以在提供給的 JavaScript 物件中提供這些選項 `withUrl` ：</span><span class="sxs-lookup"><span data-stu-id="87bb6-1266">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="87bb6-1267">在 JAVA 用戶端中，您可以使用所傳回之上的方法來設定這些選項。 `HttpHubConnectionBuilder``HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="87bb6-1267">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/chathub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="87bb6-1268">其他資源</span><span class="sxs-lookup"><span data-stu-id="87bb6-1268">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
