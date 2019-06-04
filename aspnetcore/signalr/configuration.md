---
title: ASP.NET Core SignalR 組態
author: bradygaster
description: 了解如何設定 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/03/2019
uid: signalr/configuration
ms.openlocfilehash: 6c7bd602e621917c491bfb1e26ff0fcfc3a565b0
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470363"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="fd58a-103">ASP.NET Core SignalR 組態</span><span class="sxs-lookup"><span data-stu-id="fd58a-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="fd58a-104">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="fd58a-105">ASP.NET Core SignalR 支援兩種通訊協定，針對訊息進行編碼：[JSON](https://www.json.org/)並[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="fd58a-106">每個通訊協定有序列化組態選項。</span><span class="sxs-lookup"><span data-stu-id="fd58a-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="fd58a-107">可以在 伺服器設定 JSON 序列化[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，可以在之後加入[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)中您`Startup.ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="fd58a-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="fd58a-108">`AddJsonProtocol`方法採用委派來接收`options`物件。</span><span class="sxs-lookup"><span data-stu-id="fd58a-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="fd58a-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性，該物件是 JSON.NET`JsonSerializerSettings`可用來設定序列化的引數和傳回值的物件。</span><span class="sxs-lookup"><span data-stu-id="fd58a-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="fd58a-110">請參閱[JSON.NET 文件](https://www.newtonsoft.com/json/help/html/Introduction.htm)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="fd58a-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="fd58a-111">例如，若要設定序列化程式使用 「 PascalCase"的屬性名稱，而不是預設的 「 camelCase 」 名稱，使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="fd58a-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="fd58a-112">在.NET 用戶端，相同`AddJsonProtocol`延伸模組方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="fd58a-113">`Microsoft.Extensions.DependencyInjection`必須匯入命名空間，若要解決的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="fd58a-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="fd58a-114">您不可能進行 JSON 序列化的 JavaScript 用戶端中，這一次。</span><span class="sxs-lookup"><span data-stu-id="fd58a-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="fd58a-115">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-115">MessagePack serialization options</span></span>

<span data-ttu-id="fd58a-116">您可以設定 MessagePack 序列化提供的委派[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫。</span><span class="sxs-lookup"><span data-stu-id="fd58a-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="fd58a-117">請參閱[signalr MessagePack](xref:signalr/messagepackhubprotocol)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="fd58a-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="fd58a-118">您不可以在 JavaScript 用戶端設定 MessagePack 序列化，這一次。</span><span class="sxs-lookup"><span data-stu-id="fd58a-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="fd58a-119">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-119">Configure server options</span></span>

<span data-ttu-id="fd58a-120">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="fd58a-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="fd58a-121">選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-121">Option</span></span> | <span data-ttu-id="fd58a-122">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-122">Default Value</span></span> | <span data-ttu-id="fd58a-123">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="fd58a-124">30 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-124">30 seconds</span></span> | <span data-ttu-id="fd58a-125">伺服器會考慮在用戶端已中斷連線，如果它未在此時間間隔中收到訊息 （包括為 keep-alive）。</span><span class="sxs-lookup"><span data-stu-id="fd58a-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="fd58a-126">可能需要超過用戶端實際上會標示為已中斷連線，因為這如何實作此逾時間隔。</span><span class="sxs-lookup"><span data-stu-id="fd58a-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="fd58a-127">建議的值是雙精度浮點`KeepAliveInterval`值。</span><span class="sxs-lookup"><span data-stu-id="fd58a-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="fd58a-128">15 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-128">15 seconds</span></span> | <span data-ttu-id="fd58a-129">如果用戶端不在此時間間隔內傳送初始信號交換訊息，就會關閉連線。</span><span class="sxs-lookup"><span data-stu-id="fd58a-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="fd58a-130">這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fd58a-131">如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="fd58a-132">15 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-132">15 seconds</span></span> | <span data-ttu-id="fd58a-133">如果伺服器尚未在此間隔內傳送一則訊息，是自動的 ping 訊息傳送至保持開啟的連接。</span><span class="sxs-lookup"><span data-stu-id="fd58a-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="fd58a-134">變更時`KeepAliveInterval`，變更`ServerTimeout` / `serverTimeoutInMilliseconds`設定用戶端上。</span><span class="sxs-lookup"><span data-stu-id="fd58a-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="fd58a-135">建議`ServerTimeout` / `serverTimeoutInMilliseconds`的值是雙精度浮點`KeepAliveInterval`值。</span><span class="sxs-lookup"><span data-stu-id="fd58a-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="fd58a-136">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="fd58a-136">All installed protocols</span></span> | <span data-ttu-id="fd58a-137">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-137">Protocols supported by this hub.</span></span> <span data-ttu-id="fd58a-138">根據預設，在伺服器上註冊的所有通訊協定，但可以從這個清單，以停用個別的中樞的特定通訊協定中移除通訊協定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="fd58a-139">如果`true`詳細例外狀況訊息傳回給用戶端中樞方法中擲回例外狀況時。</span><span class="sxs-lookup"><span data-stu-id="fd58a-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="fd58a-140">預設值是`false`，因為這些例外狀況訊息可以包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="fd58a-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="fd58a-141">最大用戶端就可以緩衝處理的項目上傳的資料流。</span><span class="sxs-lookup"><span data-stu-id="fd58a-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="fd58a-142">如果達到這個限制時，伺服器處理資料流項目之前封鎖引動過程的處理。</span><span class="sxs-lookup"><span data-stu-id="fd58a-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

| <span data-ttu-id="fd58a-143">選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-143">Option</span></span> | <span data-ttu-id="fd58a-144">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-144">Default Value</span></span> | <span data-ttu-id="fd58a-145">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-145">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="fd58a-146">30 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-146">30 seconds</span></span> | <span data-ttu-id="fd58a-147">伺服器會考慮在用戶端已中斷連線，如果它未在此時間間隔中收到訊息 （包括為 keep-alive）。</span><span class="sxs-lookup"><span data-stu-id="fd58a-147">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="fd58a-148">可能需要超過用戶端實際上會標示為已中斷連線，因為這如何實作此逾時間隔。</span><span class="sxs-lookup"><span data-stu-id="fd58a-148">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="fd58a-149">建議的值是雙精度浮點`KeepAliveInterval`值。</span><span class="sxs-lookup"><span data-stu-id="fd58a-149">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="fd58a-150">15 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-150">15 seconds</span></span> | <span data-ttu-id="fd58a-151">如果用戶端不在此時間間隔內傳送初始信號交換訊息，就會關閉連線。</span><span class="sxs-lookup"><span data-stu-id="fd58a-151">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="fd58a-152">這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-152">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fd58a-153">如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-153">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="fd58a-154">15 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-154">15 seconds</span></span> | <span data-ttu-id="fd58a-155">如果伺服器尚未在此間隔內傳送一則訊息，是自動的 ping 訊息傳送至保持開啟的連接。</span><span class="sxs-lookup"><span data-stu-id="fd58a-155">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="fd58a-156">變更時`KeepAliveInterval`，變更`ServerTimeout` / `serverTimeoutInMilliseconds`設定用戶端上。</span><span class="sxs-lookup"><span data-stu-id="fd58a-156">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="fd58a-157">建議`ServerTimeout` / `serverTimeoutInMilliseconds`的值是雙精度浮點`KeepAliveInterval`值。</span><span class="sxs-lookup"><span data-stu-id="fd58a-157">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="fd58a-158">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="fd58a-158">All installed protocols</span></span> | <span data-ttu-id="fd58a-159">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-159">Protocols supported by this hub.</span></span> <span data-ttu-id="fd58a-160">根據預設，在伺服器上註冊的所有通訊協定，但可以從這個清單，以停用個別的中樞的特定通訊協定中移除通訊協定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-160">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="fd58a-161">如果`true`詳細例外狀況訊息傳回給用戶端中樞方法中擲回例外狀況時。</span><span class="sxs-lookup"><span data-stu-id="fd58a-161">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="fd58a-162">預設值是`false`，因為這些例外狀況訊息可以包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="fd58a-162">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="fd58a-163">可以針對所有中樞設定選項，藉由提供選項委派`AddSignalR`呼叫中`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="fd58a-163">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="fd58a-164">單一中樞的選項會覆寫中提供的全域選項`AddSignalR`，並可使用設定<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="fd58a-164">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="fd58a-165">進階的 HTTP 組態選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-165">Advanced HTTP configuration options</span></span>

<span data-ttu-id="fd58a-166">使用`HttpConnectionDispatcherOptions`設定傳輸及記憶體緩衝區管理相關的進階的設定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-166">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="fd58a-167">這些選項會設定由傳遞至委派[MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)在`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="fd58a-167">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="fd58a-168">下表說明用於設定 ASP.NET Core SignalR 的進階的 HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="fd58a-168">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="fd58a-169">選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-169">Option</span></span> | <span data-ttu-id="fd58a-170">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-170">Default Value</span></span> | <span data-ttu-id="fd58a-171">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-171">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="fd58a-172">32 KB</span><span class="sxs-lookup"><span data-stu-id="fd58a-172">32 KB</span></span> | <span data-ttu-id="fd58a-173">從用戶端接收的位元組數目上限，伺服器緩衝區。</span><span class="sxs-lookup"><span data-stu-id="fd58a-173">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="fd58a-174">增加此值可讓伺服器接收較大的訊息，但可能會造成負面影響記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="fd58a-174">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="fd58a-175">從自動收集資料`Authorize`套用至 Hub 類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="fd58a-175">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="fd58a-176">一份[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件用來判斷是否授權用戶端來連線至中樞。</span><span class="sxs-lookup"><span data-stu-id="fd58a-176">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="fd58a-177">32 KB</span><span class="sxs-lookup"><span data-stu-id="fd58a-177">32 KB</span></span> | <span data-ttu-id="fd58a-178">應用程式所傳送的位元組數目上限，伺服器緩衝區。</span><span class="sxs-lookup"><span data-stu-id="fd58a-178">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="fd58a-179">增加此值可讓伺服器傳送較大的訊息，但可能會造成負面影響記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="fd58a-179">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="fd58a-180">會啟用所有的傳輸。</span><span class="sxs-lookup"><span data-stu-id="fd58a-180">All Transports are enabled.</span></span> | <span data-ttu-id="fd58a-181">位元遮罩`HttpTransportType`值，可以限制傳輸用戶端可用來連接。</span><span class="sxs-lookup"><span data-stu-id="fd58a-181">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="fd58a-182">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="fd58a-182">See below.</span></span> | <span data-ttu-id="fd58a-183">特有的長輪詢傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="fd58a-183">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="fd58a-184">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="fd58a-184">See below.</span></span> | <span data-ttu-id="fd58a-185">Websocket 傳輸特有的其他選項。</span><span class="sxs-lookup"><span data-stu-id="fd58a-185">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="fd58a-186">長輪詢傳輸的其他選項，您可以使用設定`LongPolling`屬性：</span><span class="sxs-lookup"><span data-stu-id="fd58a-186">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="fd58a-187">選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-187">Option</span></span> | <span data-ttu-id="fd58a-188">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-188">Default Value</span></span> | <span data-ttu-id="fd58a-189">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-189">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="fd58a-190">90 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-190">90 seconds</span></span> | <span data-ttu-id="fd58a-191">伺服器等候訊息傳送至用戶端終止單一的輪詢要求之前的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="fd58a-191">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="fd58a-192">減少此值會導致更頻繁地發出新的輪詢要求用戶端。</span><span class="sxs-lookup"><span data-stu-id="fd58a-192">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="fd58a-193">WebSocket 傳輸具有您可以使用設定的其他選項`WebSockets`屬性：</span><span class="sxs-lookup"><span data-stu-id="fd58a-193">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="fd58a-194">選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-194">Option</span></span> | <span data-ttu-id="fd58a-195">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-195">Default Value</span></span> | <span data-ttu-id="fd58a-196">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-196">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="fd58a-197">5 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-197">5 seconds</span></span> | <span data-ttu-id="fd58a-198">伺服器關閉，如果用戶端無法在此時間間隔內關閉後，會結束連接。</span><span class="sxs-lookup"><span data-stu-id="fd58a-198">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="fd58a-199">委派，可以用來設定`Sec-WebSocket-Protocol`為某個自訂值的標頭。</span><span class="sxs-lookup"><span data-stu-id="fd58a-199">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="fd58a-200">委派會接收要求的用戶端，做為輸入值，並預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="fd58a-200">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="fd58a-201">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-201">Configure client options</span></span>

<span data-ttu-id="fd58a-202">可設定上的用戶端選項`HubConnectionBuilder`（適用於.NET 和 JavaScript 的用戶端） 的型別。</span><span class="sxs-lookup"><span data-stu-id="fd58a-202">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="fd58a-203">它也會提供的 Java 用戶端，但`HttpHubConnectionBuilder`子類別是在包含的產生器的組態選項，以及`HubConnection`本身。</span><span class="sxs-lookup"><span data-stu-id="fd58a-203">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="fd58a-204">設定記錄</span><span class="sxs-lookup"><span data-stu-id="fd58a-204">Configure logging</span></span>

<span data-ttu-id="fd58a-205">記錄已在使用.NET 用戶端中`ConfigureLogging`方法。</span><span class="sxs-lookup"><span data-stu-id="fd58a-205">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="fd58a-206">記錄提供者和篩選可以註冊在相同的方式保持不在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="fd58a-206">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="fd58a-207">請參閱[ASP.NET Core 中的記錄](xref:fundamentals/logging/index)文件的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="fd58a-207">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="fd58a-208">若要註冊的記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="fd58a-208">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="fd58a-209">請參閱[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)區段的文件，如需完整清單。</span><span class="sxs-lookup"><span data-stu-id="fd58a-209">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="fd58a-210">例如，若要啟用主控台記錄，請安裝`Microsoft.Extensions.Logging.Console`NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="fd58a-210">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="fd58a-211">呼叫`AddConsole`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="fd58a-211">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="fd58a-212">在 JavaScript 用戶端，類似`configureLogging`方法存在。</span><span class="sxs-lookup"><span data-stu-id="fd58a-212">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="fd58a-213">提供`LogLevel`值，指出產生的記錄檔訊息的最低層級。</span><span class="sxs-lookup"><span data-stu-id="fd58a-213">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="fd58a-214">記錄會寫入至瀏覽器的 [主控台] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="fd58a-214">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fd58a-215">而不是`LogLevel`值，您也可以提供`string`值，表示記錄層級的名稱。</span><span class="sxs-lookup"><span data-stu-id="fd58a-215">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="fd58a-216">設定記錄在環境中的 SignalR 中並沒有存取權時，這是很有用`LogLevel`常數。</span><span class="sxs-lookup"><span data-stu-id="fd58a-216">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="fd58a-217">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="fd58a-217">The following table lists the available log levels.</span></span> <span data-ttu-id="fd58a-218">您所提供的值`configureLogging`設定**最小**記錄所記錄的層級。</span><span class="sxs-lookup"><span data-stu-id="fd58a-218">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="fd58a-219">在此層級，記錄的訊息 **，或之後它列出資料表中的層級**，將會記錄。</span><span class="sxs-lookup"><span data-stu-id="fd58a-219">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="fd58a-220">字串</span><span class="sxs-lookup"><span data-stu-id="fd58a-220">string</span></span> | <span data-ttu-id="fd58a-221">LogLevel</span><span class="sxs-lookup"><span data-stu-id="fd58a-221">LogLevel</span></span> |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| <span data-ttu-id="fd58a-222">`"info"` **或** `"information"`</span><span class="sxs-lookup"><span data-stu-id="fd58a-222">`"info"` **or** `"information"`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="fd58a-223">`"warn"` **或** `"warning"`</span><span class="sxs-lookup"><span data-stu-id="fd58a-223">`"warn"` **or** `"warning"`</span></span> | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="fd58a-224">若要停用整個記錄，請指定`signalR.LogLevel.None`在`configureLogging`方法。</span><span class="sxs-lookup"><span data-stu-id="fd58a-224">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="fd58a-225">如需有關記錄的詳細資訊，請參閱 < [SignalR 診斷文件](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-225">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="fd58a-226">SignalR Java 用戶端會使用[SLF4J](https://www.slf4j.org/)記錄的程式庫。</span><span class="sxs-lookup"><span data-stu-id="fd58a-226">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="fd58a-227">它是高層級的記錄 API，可讓程式庫的使用者選擇他們自己的特定記錄實作，藉由將特定記錄相依性。</span><span class="sxs-lookup"><span data-stu-id="fd58a-227">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="fd58a-228">下列程式碼片段示範如何使用`java.util.logging`與 SignalR Java 用戶端。</span><span class="sxs-lookup"><span data-stu-id="fd58a-228">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="fd58a-229">如果您未設定登入您的相依性，SLF4J 會載入預設的無作業記錄器，並出現下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="fd58a-229">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="fd58a-230">這可以放心地忽略。</span><span class="sxs-lookup"><span data-stu-id="fd58a-230">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="fd58a-231">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="fd58a-231">Configure allowed transports</span></span>

<span data-ttu-id="fd58a-232">SignalR 使用的傳輸可以設定為在`WithUrl`呼叫 (`withUrl`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-232">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="fd58a-233">位元 OR 之值的`HttpTransportType`可用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="fd58a-233">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="fd58a-234">預設會啟用所有的傳輸。</span><span class="sxs-lookup"><span data-stu-id="fd58a-234">All transports are enabled by default.</span></span>

<span data-ttu-id="fd58a-235">例如，若要停用 Server-Sent 事件傳輸，但允許 Websocket 及長輪詢連線：</span><span class="sxs-lookup"><span data-stu-id="fd58a-235">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="fd58a-236">在 JavaScript 用戶端，透過設定來設定傳輸`transport`提供給選項物件欄位`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fd58a-236">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fd58a-237">在這個版本的 Java 用戶端 websocket 會是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="fd58a-237">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="fd58a-238">中的 Java 用戶端中，選取傳輸與`withTransport`方法`HttpHubConnectionBuilder`。</span><span class="sxs-lookup"><span data-stu-id="fd58a-238">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="fd58a-239">Java 用戶端預設會使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="fd58a-239">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="fd58a-240">SignalR Java 用戶端尚不支援傳輸後援。</span><span class="sxs-lookup"><span data-stu-id="fd58a-240">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="fd58a-241">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="fd58a-241">Configure bearer authentication</span></span>

<span data-ttu-id="fd58a-242">若要提供與 SignalR 要求的驗證資料，請使用`AccessTokenProvider`選項 (`accessTokenFactory`在 JavaScript 中) 來指定傳回的所需的存取權杖的函式。</span><span class="sxs-lookup"><span data-stu-id="fd58a-242">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="fd58a-243">在.NET 用戶端，此存取權杖會傳遞為 HTTP 「 持有人驗證 」 權杖 (使用`Authorization`類型的標頭`Bearer`)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-243">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="fd58a-244">在 JavaScript 用戶端的存取權杖做為持有人權杖，**除了**在少數情況下，瀏覽器 Api 限制能夠套用標頭 （特別是，在 Server-Sent 事件和 Websocket 要求）。</span><span class="sxs-lookup"><span data-stu-id="fd58a-244">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="fd58a-245">在這些情況下，存取權杖中提供的查詢字串值`access_token`。</span><span class="sxs-lookup"><span data-stu-id="fd58a-245">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="fd58a-246">在.NET 用戶端`AccessTokenProvider`可以指定選項，使用中的選項委派`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="fd58a-246">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="fd58a-247">在 JavaScript 用戶端，透過設定來設定存取權杖`accessTokenFactory`欄位中的選項物件`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fd58a-247">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="fd58a-248">SignalR Java 用戶端，在中，您可以設定要用於驗證所提供的存取語彙基元 factory，以持有人權杖[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-248">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="fd58a-249">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJava](https://github.com/ReactiveX/RxJava) [單一\<字串 >](http://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-249">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="fd58a-250">藉由呼叫[Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您可以撰寫邏輯，以針對您的用戶端產生存取權杖。</span><span class="sxs-lookup"><span data-stu-id="fd58a-250">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="fd58a-251">設定逾時和保持連線選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-251">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="fd58a-252">設定逾時和持續連線行為的其他選項可供使用`HubConnection`物件本身：</span><span class="sxs-lookup"><span data-stu-id="fd58a-252">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="fd58a-253">.NET</span><span class="sxs-lookup"><span data-stu-id="fd58a-253">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="fd58a-254">選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-254">Option</span></span> | <span data-ttu-id="fd58a-255">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-255">Default value</span></span> | <span data-ttu-id="fd58a-256">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-256">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="fd58a-257">30 秒 （30,000 毫秒）</span><span class="sxs-lookup"><span data-stu-id="fd58a-257">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="fd58a-258">伺服器活動的逾時。</span><span class="sxs-lookup"><span data-stu-id="fd58a-258">Timeout for server activity.</span></span> <span data-ttu-id="fd58a-259">如果伺服器未傳送訊息，此時間間隔中，用戶端會視為中斷連線的 server 和觸發程序`Closed`事件 (`onclose`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-259">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="fd58a-260">此值必須夠大，從伺服器傳送的 ping 訊息**和**逾時間隔內收到用戶端。</span><span class="sxs-lookup"><span data-stu-id="fd58a-260">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="fd58a-261">建議的值是數字在至少兩倍的伺服器的`KeepAliveInterval`值，以允許 ping 抵達的時間。</span><span class="sxs-lookup"><span data-stu-id="fd58a-261">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="fd58a-262">15 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-262">15 seconds</span></span> | <span data-ttu-id="fd58a-263">初始伺服器交握的逾時。</span><span class="sxs-lookup"><span data-stu-id="fd58a-263">Timeout for initial server handshake.</span></span> <span data-ttu-id="fd58a-264">如果伺服器不會傳送交握回應此時間間隔中，用戶端便會取消交握和觸發程序`Closed`事件 (`onclose`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-264">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="fd58a-265">這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-265">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fd58a-266">如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-266">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="fd58a-267">在.NET 用戶端逾時的值會指定為`TimeSpan`值。</span><span class="sxs-lookup"><span data-stu-id="fd58a-267">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="fd58a-268">JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd58a-268">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="fd58a-269">選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-269">Option</span></span> | <span data-ttu-id="fd58a-270">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-270">Default value</span></span> | <span data-ttu-id="fd58a-271">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-271">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="fd58a-272">30 秒 （30,000 毫秒）</span><span class="sxs-lookup"><span data-stu-id="fd58a-272">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="fd58a-273">伺服器活動的逾時。</span><span class="sxs-lookup"><span data-stu-id="fd58a-273">Timeout for server activity.</span></span> <span data-ttu-id="fd58a-274">如果伺服器未傳送訊息，此時間間隔中，用戶端會視為中斷連線的 server 和觸發程序`onclose`事件。</span><span class="sxs-lookup"><span data-stu-id="fd58a-274">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="fd58a-275">此值必須夠大，從伺服器傳送的 ping 訊息**和**逾時間隔內收到用戶端。</span><span class="sxs-lookup"><span data-stu-id="fd58a-275">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="fd58a-276">建議的值是數字在至少兩倍的伺服器的`KeepAliveInterval`值，以允許 ping 抵達的時間。</span><span class="sxs-lookup"><span data-stu-id="fd58a-276">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="fd58a-277">Java</span><span class="sxs-lookup"><span data-stu-id="fd58a-277">Java</span></span>](#tab/java)

| <span data-ttu-id="fd58a-278">選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-278">Option</span></span> | <span data-ttu-id="fd58a-279">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-279">Default value</span></span> | <span data-ttu-id="fd58a-280">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-280">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="fd58a-281">`getServerTimeout` `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="fd58a-281">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="fd58a-282">30 秒 （30,000 毫秒）</span><span class="sxs-lookup"><span data-stu-id="fd58a-282">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="fd58a-283">伺服器活動的逾時。</span><span class="sxs-lookup"><span data-stu-id="fd58a-283">Timeout for server activity.</span></span> <span data-ttu-id="fd58a-284">如果伺服器未傳送訊息，此時間間隔中，用戶端會視為中斷連線的 server 和觸發程序`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="fd58a-284">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="fd58a-285">此值必須夠大，從伺服器傳送的 ping 訊息**和**逾時間隔內收到用戶端。</span><span class="sxs-lookup"><span data-stu-id="fd58a-285">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="fd58a-286">建議的值是數字在至少兩倍的伺服器的`KeepAliveInterval`值，以允許 ping 抵達的時間。</span><span class="sxs-lookup"><span data-stu-id="fd58a-286">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="fd58a-287">15 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-287">15 seconds</span></span> | <span data-ttu-id="fd58a-288">初始伺服器交握的逾時。</span><span class="sxs-lookup"><span data-stu-id="fd58a-288">Timeout for initial server handshake.</span></span> <span data-ttu-id="fd58a-289">如果伺服器不會傳送交握回應此時間間隔中，用戶端便會取消交握和觸發程序`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="fd58a-289">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="fd58a-290">這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-290">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fd58a-291">如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="fd58a-291">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="fd58a-292">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-292">Configure additional options</span></span>

<span data-ttu-id="fd58a-293">可以設定其他選項`WithUrl`(`withUrl`在 JavaScript 中) 上的方法`HubConnectionBuilder`或各種組態 Api 上`HttpHubConnectionBuilder`中的 Java 用戶端：</span><span class="sxs-lookup"><span data-stu-id="fd58a-293">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="fd58a-294">.NET</span><span class="sxs-lookup"><span data-stu-id="fd58a-294">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="fd58a-295">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-295">.NET Option</span></span> |  <span data-ttu-id="fd58a-296">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-296">Default value</span></span> | <span data-ttu-id="fd58a-297">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-297">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="fd58a-298">傳回字串，做為持有人驗證權杖，在 HTTP 要求中提供的函式。</span><span class="sxs-lookup"><span data-stu-id="fd58a-298">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="fd58a-299">將此設為`true`略過交涉步驟。</span><span class="sxs-lookup"><span data-stu-id="fd58a-299">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="fd58a-300">**Websocket 傳輸方式是只啟用的傳輸時，才支援**。</span><span class="sxs-lookup"><span data-stu-id="fd58a-300">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="fd58a-301">使用 Azure SignalR 服務時，就無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-301">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="fd58a-302">Empty</span><span class="sxs-lookup"><span data-stu-id="fd58a-302">Empty</span></span> | <span data-ttu-id="fd58a-303">傳送驗證要求的 TLS 憑證的集合。</span><span class="sxs-lookup"><span data-stu-id="fd58a-303">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="fd58a-304">Empty</span><span class="sxs-lookup"><span data-stu-id="fd58a-304">Empty</span></span> | <span data-ttu-id="fd58a-305">要與每個 HTTP 要求一起傳送的 HTTP cookie 的集合。</span><span class="sxs-lookup"><span data-stu-id="fd58a-305">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="fd58a-306">Empty</span><span class="sxs-lookup"><span data-stu-id="fd58a-306">Empty</span></span> | <span data-ttu-id="fd58a-307">要與每個 HTTP 要求一起傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="fd58a-307">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="fd58a-308">5 秒</span><span class="sxs-lookup"><span data-stu-id="fd58a-308">5 seconds</span></span> | <span data-ttu-id="fd58a-309">只有 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="fd58a-309">WebSockets only.</span></span> <span data-ttu-id="fd58a-310">最大時間量，該用戶端會等待伺服器以確認在關閉要求的結尾後面。</span><span class="sxs-lookup"><span data-stu-id="fd58a-310">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="fd58a-311">如果伺服器不在此時間內認可關閉，則用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="fd58a-311">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="fd58a-312">Empty</span><span class="sxs-lookup"><span data-stu-id="fd58a-312">Empty</span></span> | <span data-ttu-id="fd58a-313">要與每個 HTTP 要求一起傳送的其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="fd58a-313">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="fd58a-314">委派，可用來設定或取代`HttpMessageHandler`用來傳送 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="fd58a-314">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="fd58a-315">不使用 WebSocket 連線。</span><span class="sxs-lookup"><span data-stu-id="fd58a-315">Not used for WebSocket connections.</span></span> <span data-ttu-id="fd58a-316">此委派必須傳回非 null 值，並且會收到做為參數的預設值。</span><span class="sxs-lookup"><span data-stu-id="fd58a-316">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="fd58a-317">修改設定，該預設值，並傳回它，或傳回新`HttpMessageHandler`執行個體。</span><span class="sxs-lookup"><span data-stu-id="fd58a-317">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="fd58a-318">**當複製您想要保留從提供的處理常式的設定，請務必取代處理常式，否則設定的選項 （例如 Cookie 和標頭） 不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="fd58a-318">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="fd58a-319">傳送 HTTP 要求時要使用 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="fd58a-319">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="fd58a-320">設定這個傳送 HTTP 和 Websocket 要求的預設認證的布林值。</span><span class="sxs-lookup"><span data-stu-id="fd58a-320">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="fd58a-321">這可讓使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="fd58a-321">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="fd58a-322">委派，可用來設定其他的 WebSocket 選項。</span><span class="sxs-lookup"><span data-stu-id="fd58a-322">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="fd58a-323">收到的執行個體[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) ，可用來設定選項。</span><span class="sxs-lookup"><span data-stu-id="fd58a-323">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="fd58a-324">JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd58a-324">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="fd58a-325">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-325">JavaScript Option</span></span> | <span data-ttu-id="fd58a-326">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-326">Default Value</span></span> | <span data-ttu-id="fd58a-327">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-327">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="fd58a-328">傳回字串，做為持有人驗證權杖，在 HTTP 要求中提供的函式。</span><span class="sxs-lookup"><span data-stu-id="fd58a-328">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="fd58a-329">將此設為`true`略過交涉步驟。</span><span class="sxs-lookup"><span data-stu-id="fd58a-329">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="fd58a-330">**Websocket 傳輸方式是只啟用的傳輸時，才支援**。</span><span class="sxs-lookup"><span data-stu-id="fd58a-330">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="fd58a-331">使用 Azure SignalR 服務時，就無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-331">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="fd58a-332">Java</span><span class="sxs-lookup"><span data-stu-id="fd58a-332">Java</span></span>](#tab/java)

| <span data-ttu-id="fd58a-333">Java 選項</span><span class="sxs-lookup"><span data-stu-id="fd58a-333">Java Option</span></span> | <span data-ttu-id="fd58a-334">預設值</span><span class="sxs-lookup"><span data-stu-id="fd58a-334">Default Value</span></span> | <span data-ttu-id="fd58a-335">描述</span><span class="sxs-lookup"><span data-stu-id="fd58a-335">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="fd58a-336">傳回字串，做為持有人驗證權杖，在 HTTP 要求中提供的函式。</span><span class="sxs-lookup"><span data-stu-id="fd58a-336">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="fd58a-337">將此設為`true`略過交涉步驟。</span><span class="sxs-lookup"><span data-stu-id="fd58a-337">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="fd58a-338">**Websocket 傳輸方式是只啟用的傳輸時，才支援**。</span><span class="sxs-lookup"><span data-stu-id="fd58a-338">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="fd58a-339">使用 Azure SignalR 服務時，就無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="fd58a-339">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="fd58a-340">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="fd58a-340">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="fd58a-341">Empty</span><span class="sxs-lookup"><span data-stu-id="fd58a-341">Empty</span></span> | <span data-ttu-id="fd58a-342">要與每個 HTTP 要求一起傳送的其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="fd58a-342">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="fd58a-343">在.NET 用戶端，可以修改這些選項所提供的選項委派`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="fd58a-343">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="fd58a-344">在 JavaScript 用戶端，可以提供這些選項中提供的 JavaScript 物件`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fd58a-344">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="fd58a-345">在 Java 用戶端，這些選項可以設定的方法上`HttpHubConnectionBuilder`傳回 `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="fd58a-345">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="fd58a-346">其他資源</span><span class="sxs-lookup"><span data-stu-id="fd58a-346">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
