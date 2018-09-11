---
title: ASP.NET Core SignalR 組態
author: tdykstra
description: 了解如何設定 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: b7c9c3713faa952c2b5bd142ab4887ccbc120ea2
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373367"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="72303-103">ASP.NET Core SignalR 組態</span><span class="sxs-lookup"><span data-stu-id="72303-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="72303-104">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="72303-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="72303-105">ASP.NET Core SignalR 支援兩種通訊協定，針對訊息進行編碼： [JSON](https://www.json.org/)並[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="72303-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="72303-106">每個通訊協定有序列化組態選項。</span><span class="sxs-lookup"><span data-stu-id="72303-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="72303-107">可以在 伺服器設定 JSON 序列化[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，可以在之後加入[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)中您`Startup.ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="72303-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="72303-108">`AddJsonProtocol`方法採用委派來接收`options`物件。</span><span class="sxs-lookup"><span data-stu-id="72303-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="72303-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性，該物件是 JSON.NET`JsonSerializerSettings`可用來設定序列化的引數和傳回值的物件。</span><span class="sxs-lookup"><span data-stu-id="72303-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="72303-110">請參閱[JSON.NET 文件](https://www.newtonsoft.com/json/help/html/Introduction.htm)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="72303-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="72303-111">例如，若要設定序列化程式使用 「 PascalCase"的屬性名稱，而不是預設的 「 camelCase 」 名稱，使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="72303-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="72303-112">在.NET 用戶端，相同`AddJsonProtocol`延伸模組方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)。</span><span class="sxs-lookup"><span data-stu-id="72303-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="72303-113">`Microsoft.Extensions.DependencyInjection`必須匯入命名空間，若要解決的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="72303-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
            new DefaultContractResolver();
    });
```

> [!NOTE]
> <span data-ttu-id="72303-114">您不可能進行 JSON 序列化的 JavaScript 用戶端中，這一次。</span><span class="sxs-lookup"><span data-stu-id="72303-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="72303-115">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="72303-115">MessagePack serialization options</span></span>

<span data-ttu-id="72303-116">您可以設定 MessagePack 序列化提供的委派[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫。</span><span class="sxs-lookup"><span data-stu-id="72303-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="72303-117">請參閱[signalr MessagePack](xref:signalr/messagepackhubprotocol)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="72303-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="72303-118">您不可以在 JavaScript 用戶端設定 MessagePack 序列化，這一次。</span><span class="sxs-lookup"><span data-stu-id="72303-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="72303-119">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="72303-119">Configure server options</span></span>

<span data-ttu-id="72303-120">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="72303-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="72303-121">選項</span><span class="sxs-lookup"><span data-stu-id="72303-121">Option</span></span> | <span data-ttu-id="72303-122">預設值</span><span class="sxs-lookup"><span data-stu-id="72303-122">Default Value</span></span> | <span data-ttu-id="72303-123">描述</span><span class="sxs-lookup"><span data-stu-id="72303-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="72303-124">15 秒</span><span class="sxs-lookup"><span data-stu-id="72303-124">15 seconds</span></span> | <span data-ttu-id="72303-125">如果用戶端不在此時間間隔內傳送初始信號交換訊息，就會關閉連線。</span><span class="sxs-lookup"><span data-stu-id="72303-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="72303-126">這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。</span><span class="sxs-lookup"><span data-stu-id="72303-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72303-127">如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="72303-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="72303-128">15 秒</span><span class="sxs-lookup"><span data-stu-id="72303-128">15 seconds</span></span> | <span data-ttu-id="72303-129">如果伺服器尚未在此間隔內傳送一則訊息，是自動的 ping 訊息傳送至保持開啟的連接。</span><span class="sxs-lookup"><span data-stu-id="72303-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="72303-130">變更時`KeepAliveInterval`，變更`ServerTimeout` / `serverTimeoutInMilliseconds`設定用戶端上。</span><span class="sxs-lookup"><span data-stu-id="72303-130">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="72303-131">建議`ServerTimeout` / `serverTimeoutInMilliseconds`的值是雙精度浮點`KeepAliveInterval`值。</span><span class="sxs-lookup"><span data-stu-id="72303-131">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="72303-132">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="72303-132">All installed protocols</span></span> | <span data-ttu-id="72303-133">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="72303-133">Protocols supported by this hub.</span></span> <span data-ttu-id="72303-134">根據預設，在伺服器上註冊的所有通訊協定，但可以從這個清單，以停用個別的中樞的特定通訊協定中移除通訊協定。</span><span class="sxs-lookup"><span data-stu-id="72303-134">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="72303-135">如果`true`詳細例外狀況訊息傳回給用戶端中樞方法中擲回例外狀況時。</span><span class="sxs-lookup"><span data-stu-id="72303-135">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="72303-136">預設值是`false`，因為這些例外狀況訊息可以包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="72303-136">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="72303-137">可以針對所有中樞設定選項，藉由提供選項委派`AddSignalR`呼叫中`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="72303-137">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="72303-138">單一中樞的選項會覆寫中提供的全域選項`AddSignalR`，並可使用設定[AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="72303-138">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="72303-139">使用`HttpConnectionDispatcherOptions`設定傳輸及記憶體緩衝區管理相關的進階的設定。</span><span class="sxs-lookup"><span data-stu-id="72303-139">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="72303-140">這些選項會設定由傳遞至委派[MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)。</span><span class="sxs-lookup"><span data-stu-id="72303-140">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="72303-141">選項</span><span class="sxs-lookup"><span data-stu-id="72303-141">Option</span></span> | <span data-ttu-id="72303-142">預設值</span><span class="sxs-lookup"><span data-stu-id="72303-142">Default Value</span></span> | <span data-ttu-id="72303-143">描述</span><span class="sxs-lookup"><span data-stu-id="72303-143">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="72303-144">32 KB</span><span class="sxs-lookup"><span data-stu-id="72303-144">32 KB</span></span> | <span data-ttu-id="72303-145">從用戶端接收的位元組數目上限，伺服器緩衝區。</span><span class="sxs-lookup"><span data-stu-id="72303-145">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="72303-146">增加此值可讓伺服器接收較大的訊息，但可能會造成負面影響記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="72303-146">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="72303-147">從自動收集資料`Authorize`套用至 Hub 類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="72303-147">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="72303-148">一份[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件用來判斷是否授權用戶端來連線至中樞。</span><span class="sxs-lookup"><span data-stu-id="72303-148">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="72303-149">32 KB</span><span class="sxs-lookup"><span data-stu-id="72303-149">32 KB</span></span> | <span data-ttu-id="72303-150">應用程式所傳送的位元組數目上限，伺服器緩衝區。</span><span class="sxs-lookup"><span data-stu-id="72303-150">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="72303-151">增加此值可讓伺服器傳送較大的訊息，但可能會造成負面影響記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="72303-151">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="72303-152">會啟用所有的傳輸。</span><span class="sxs-lookup"><span data-stu-id="72303-152">All Transports are enabled.</span></span> | <span data-ttu-id="72303-153">位元遮罩`HttpTransportType`值，可以限制傳輸用戶端可用來連接。</span><span class="sxs-lookup"><span data-stu-id="72303-153">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="72303-154">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="72303-154">See below.</span></span> | <span data-ttu-id="72303-155">特有的長輪詢傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="72303-155">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="72303-156">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="72303-156">See below.</span></span> | <span data-ttu-id="72303-157">Websocket 傳輸特有的其他選項。</span><span class="sxs-lookup"><span data-stu-id="72303-157">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="72303-158">長輪詢傳輸的其他選項，您可以使用設定`LongPolling`屬性：</span><span class="sxs-lookup"><span data-stu-id="72303-158">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="72303-159">選項</span><span class="sxs-lookup"><span data-stu-id="72303-159">Option</span></span> | <span data-ttu-id="72303-160">預設值</span><span class="sxs-lookup"><span data-stu-id="72303-160">Default Value</span></span> | <span data-ttu-id="72303-161">描述</span><span class="sxs-lookup"><span data-stu-id="72303-161">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="72303-162">90 秒</span><span class="sxs-lookup"><span data-stu-id="72303-162">90 seconds</span></span> | <span data-ttu-id="72303-163">伺服器等候訊息傳送至用戶端終止單一的輪詢要求之前的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="72303-163">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="72303-164">減少此值會導致更頻繁地發出新的輪詢要求用戶端。</span><span class="sxs-lookup"><span data-stu-id="72303-164">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="72303-165">WebSocket 傳輸具有您可以使用設定的其他選項`WebSockets`屬性：</span><span class="sxs-lookup"><span data-stu-id="72303-165">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="72303-166">選項</span><span class="sxs-lookup"><span data-stu-id="72303-166">Option</span></span> | <span data-ttu-id="72303-167">預設值</span><span class="sxs-lookup"><span data-stu-id="72303-167">Default Value</span></span> | <span data-ttu-id="72303-168">描述</span><span class="sxs-lookup"><span data-stu-id="72303-168">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="72303-169">5 秒</span><span class="sxs-lookup"><span data-stu-id="72303-169">5 seconds</span></span> | <span data-ttu-id="72303-170">伺服器關閉，如果用戶端無法在此時間間隔內關閉後，會結束連接。</span><span class="sxs-lookup"><span data-stu-id="72303-170">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="72303-171">委派，可以用來設定`Sec-WebSocket-Protocol`為某個自訂值的標頭。</span><span class="sxs-lookup"><span data-stu-id="72303-171">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="72303-172">委派會接收要求的用戶端，做為輸入值，並預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="72303-172">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="72303-173">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="72303-173">Configure client options</span></span>

<span data-ttu-id="72303-174">可設定上的用戶端選項`HubConnectionBuilder`型別 （適用於.NET 和 JavaScript 的用戶端），以及`HubConnection`本身。</span><span class="sxs-lookup"><span data-stu-id="72303-174">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="72303-175">設定記錄</span><span class="sxs-lookup"><span data-stu-id="72303-175">Configure logging</span></span>

<span data-ttu-id="72303-176">記錄已在使用.NET 用戶端中`ConfigureLogging`方法。</span><span class="sxs-lookup"><span data-stu-id="72303-176">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="72303-177">記錄提供者和篩選可以註冊在相同的方式保持不在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="72303-177">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="72303-178">請參閱[ASP.NET Core 中的記錄](xref:fundamentals/logging/index#how-to-add-providers)文件的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="72303-178">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="72303-179">若要註冊的記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="72303-179">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="72303-180">請參閱[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)區段的文件，如需完整清單。</span><span class="sxs-lookup"><span data-stu-id="72303-180">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="72303-181">例如，若要啟用主控台記錄，請安裝`Microsoft.Extensions.Logging.Console`NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="72303-181">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="72303-182">呼叫`AddConsole`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="72303-182">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="72303-183">在 JavaScript 用戶端，類似`configureLogging`方法存在。</span><span class="sxs-lookup"><span data-stu-id="72303-183">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="72303-184">提供`LogLevel`值，指出產生的記錄檔訊息的最低層級。</span><span class="sxs-lookup"><span data-stu-id="72303-184">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="72303-185">記錄會寫入至瀏覽器的 [主控台] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="72303-185">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="72303-186">若要停用整個記錄，請指定`signalR.LogLevel.None`在`configureLogging`方法。</span><span class="sxs-lookup"><span data-stu-id="72303-186">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="72303-187">以下列出可用的 JavaScript 用戶端的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="72303-187">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="72303-188">將記錄層級設定為其中一個值可讓在訊息的記錄**或更新版本**該層級。</span><span class="sxs-lookup"><span data-stu-id="72303-188">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="72303-189">層級</span><span class="sxs-lookup"><span data-stu-id="72303-189">Level</span></span> | <span data-ttu-id="72303-190">描述</span><span class="sxs-lookup"><span data-stu-id="72303-190">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="72303-191">會不記錄任何訊息。</span><span class="sxs-lookup"><span data-stu-id="72303-191">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="72303-192">表示在整個應用程式中的失敗的訊息。</span><span class="sxs-lookup"><span data-stu-id="72303-192">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="72303-193">表示目前的作業中失敗的訊息。</span><span class="sxs-lookup"><span data-stu-id="72303-193">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="72303-194">表示非嚴重問題的訊息。</span><span class="sxs-lookup"><span data-stu-id="72303-194">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="72303-195">參考用訊息。</span><span class="sxs-lookup"><span data-stu-id="72303-195">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="72303-196">適用於偵錯的診斷訊息。</span><span class="sxs-lookup"><span data-stu-id="72303-196">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="72303-197">設計用來診斷特定問題非常詳細的診斷訊息。</span><span class="sxs-lookup"><span data-stu-id="72303-197">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="72303-198">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="72303-198">Configure allowed transports</span></span>

<span data-ttu-id="72303-199">SignalR 使用的傳輸可以設定為在`WithUrl`呼叫 (`withUrl`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="72303-199">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="72303-200">位元 OR 之值的`HttpTransportType`可用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="72303-200">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="72303-201">預設會啟用所有的傳輸。</span><span class="sxs-lookup"><span data-stu-id="72303-201">All transports are enabled by default.</span></span>

<span data-ttu-id="72303-202">例如，若要停用 Server-Sent 事件傳輸，但允許 Websocket 及長輪詢連線：</span><span class="sxs-lookup"><span data-stu-id="72303-202">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="72303-203">在 JavaScript 用戶端，透過設定來設定傳輸`transport`提供給選項物件欄位`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72303-203">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="72303-204">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="72303-204">Configure bearer authentication</span></span>

<span data-ttu-id="72303-205">若要提供與 SignalR 要求的驗證資料，請使用`AccessTokenProvider`選項 (`accessTokenFactory`在 JavaScript 中) 來指定傳回的所需的存取權杖的函式。</span><span class="sxs-lookup"><span data-stu-id="72303-205">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="72303-206">在.NET 用戶端，此存取權杖會傳遞為 HTTP 「 持有人驗證 」 權杖 (使用`Authorization`類型的標頭`Bearer`)。</span><span class="sxs-lookup"><span data-stu-id="72303-206">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="72303-207">在 JavaScript 用戶端的存取權杖做為持有人權杖，**除了**在少數情況下，瀏覽器 Api 限制能夠套用標頭 （特別是，在 Server-Sent 事件和 Websocket 要求）。</span><span class="sxs-lookup"><span data-stu-id="72303-207">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="72303-208">在這些情況下，存取權杖中提供的查詢字串值`access_token`。</span><span class="sxs-lookup"><span data-stu-id="72303-208">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="72303-209">在.NET 用戶端`AccessTokenProvider`可以指定選項，使用中的選項委派`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="72303-209">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="72303-210">在 JavaScript 用戶端，透過設定來設定存取權杖`accessTokenFactory`欄位中的選項物件`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72303-210">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="72303-211">設定逾時和保持連線選項</span><span class="sxs-lookup"><span data-stu-id="72303-211">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="72303-212">設定逾時和持續連線行為的其他選項可供使用`HubConnection`物件本身：</span><span class="sxs-lookup"><span data-stu-id="72303-212">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="72303-213">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="72303-213">.NET Option</span></span> | <span data-ttu-id="72303-214">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="72303-214">JavaScript Option</span></span> | <span data-ttu-id="72303-215">預設值</span><span class="sxs-lookup"><span data-stu-id="72303-215">Default Value</span></span> | <span data-ttu-id="72303-216">描述</span><span class="sxs-lookup"><span data-stu-id="72303-216">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="72303-217">30 秒 （30,000 毫秒）</span><span class="sxs-lookup"><span data-stu-id="72303-217">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="72303-218">伺服器活動的逾時。</span><span class="sxs-lookup"><span data-stu-id="72303-218">Timeout for server activity.</span></span> <span data-ttu-id="72303-219">如果伺服器未傳送訊息，此時間間隔中，用戶端會視為中斷連線的 server 和觸發程序`Closed`事件 (`onclose`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="72303-219">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="72303-220">此值必須夠大，從伺服器傳送的 ping 訊息**和**逾時間隔內收到用戶端。</span><span class="sxs-lookup"><span data-stu-id="72303-220">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="72303-221">建議的值是數字在至少兩倍的伺服器的`KeepAliveInterval`值，以允許 ping 抵達的時間。</span><span class="sxs-lookup"><span data-stu-id="72303-221">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="72303-222">無法設定</span><span class="sxs-lookup"><span data-stu-id="72303-222">Not configurable</span></span> | <span data-ttu-id="72303-223">15 秒</span><span class="sxs-lookup"><span data-stu-id="72303-223">15 seconds</span></span> | <span data-ttu-id="72303-224">初始伺服器交握的逾時。</span><span class="sxs-lookup"><span data-stu-id="72303-224">Timeout for initial server handshake.</span></span> <span data-ttu-id="72303-225">如果伺服器不會傳送交握回應此時間間隔中，用戶端便會取消交握和觸發程序`Closed`事件 (`onclose`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="72303-225">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="72303-226">這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。</span><span class="sxs-lookup"><span data-stu-id="72303-226">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72303-227">如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="72303-227">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="72303-228">在.NET 用戶端逾時的值會指定為`TimeSpan`值。</span><span class="sxs-lookup"><span data-stu-id="72303-228">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="72303-229">在 JavaScript 用戶端逾時的值會指定為數字，表示以毫秒為單位的持續時間。</span><span class="sxs-lookup"><span data-stu-id="72303-229">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="72303-230">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="72303-230">Configure additional options</span></span>

<span data-ttu-id="72303-231">可以設定其他選項`WithUrl`(`withUrl`在 JavaScript 中) 上的方法`HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="72303-231">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="72303-232">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="72303-232">.NET Option</span></span> | <span data-ttu-id="72303-233">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="72303-233">JavaScript Option</span></span> | <span data-ttu-id="72303-234">預設值</span><span class="sxs-lookup"><span data-stu-id="72303-234">Default Value</span></span> | <span data-ttu-id="72303-235">描述</span><span class="sxs-lookup"><span data-stu-id="72303-235">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="72303-236">傳回字串，做為持有人驗證權杖，在 HTTP 要求中提供的函式。</span><span class="sxs-lookup"><span data-stu-id="72303-236">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="72303-237">將此設為`true`略過交涉步驟。</span><span class="sxs-lookup"><span data-stu-id="72303-237">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72303-238">**Websocket 傳輸方式是只啟用的傳輸時，才支援**。</span><span class="sxs-lookup"><span data-stu-id="72303-238">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72303-239">使用 Azure SignalR 服務時，就無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="72303-239">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="72303-240">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="72303-240">Not configurable \*</span></span> | <span data-ttu-id="72303-241">Empty</span><span class="sxs-lookup"><span data-stu-id="72303-241">Empty</span></span> | <span data-ttu-id="72303-242">傳送驗證要求的 TLS 憑證的集合。</span><span class="sxs-lookup"><span data-stu-id="72303-242">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="72303-243">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="72303-243">Not configurable \*</span></span> | <span data-ttu-id="72303-244">Empty</span><span class="sxs-lookup"><span data-stu-id="72303-244">Empty</span></span> | <span data-ttu-id="72303-245">要與每個 HTTP 要求一起傳送的 HTTP cookie 的集合。</span><span class="sxs-lookup"><span data-stu-id="72303-245">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="72303-246">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="72303-246">Not configurable \*</span></span> | <span data-ttu-id="72303-247">Empty</span><span class="sxs-lookup"><span data-stu-id="72303-247">Empty</span></span> | <span data-ttu-id="72303-248">要與每個 HTTP 要求一起傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="72303-248">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="72303-249">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="72303-249">Not configurable \*</span></span> | <span data-ttu-id="72303-250">5 秒</span><span class="sxs-lookup"><span data-stu-id="72303-250">5 seconds</span></span> | <span data-ttu-id="72303-251">只有 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="72303-251">WebSockets only.</span></span> <span data-ttu-id="72303-252">最大時間量，該用戶端會等待伺服器以確認在關閉要求的結尾後面。</span><span class="sxs-lookup"><span data-stu-id="72303-252">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="72303-253">如果伺服器不在此時間內認可關閉，則用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="72303-253">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="72303-254">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="72303-254">Not configurable \*</span></span> | <span data-ttu-id="72303-255">Empty</span><span class="sxs-lookup"><span data-stu-id="72303-255">Empty</span></span> | <span data-ttu-id="72303-256">要與每個 HTTP 要求一起傳送的其他 HTTP 標頭的字典。</span><span class="sxs-lookup"><span data-stu-id="72303-256">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="72303-257">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="72303-257">Not configurable \*</span></span> | `null` | <span data-ttu-id="72303-258">委派，可用來設定或取代`HttpMessageHandler`用來傳送 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="72303-258">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="72303-259">不使用 WebSocket 連線。</span><span class="sxs-lookup"><span data-stu-id="72303-259">Not used for WebSocket connections.</span></span> <span data-ttu-id="72303-260">此委派必須傳回非 null 值，並且會收到做為參數的預設值。</span><span class="sxs-lookup"><span data-stu-id="72303-260">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="72303-261">修改設定，該預設值，並傳回它，或傳回新`HttpMessageHandler`執行個體。</span><span class="sxs-lookup"><span data-stu-id="72303-261">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="72303-262">**當複製您想要保留從提供的處理常式的設定，請務必取代處理常式，否則設定的選項 （例如 Cookie 和標頭） 不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="72303-262">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="72303-263">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="72303-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="72303-264">傳送 HTTP 要求時要使用 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="72303-264">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="72303-265">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="72303-265">Not configurable \*</span></span> | `false` | <span data-ttu-id="72303-266">設定這個傳送 HTTP 和 Websocket 要求的預設認證的布林值。</span><span class="sxs-lookup"><span data-stu-id="72303-266">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="72303-267">這可讓使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="72303-267">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="72303-268">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="72303-268">Not configurable \*</span></span> | `null` | <span data-ttu-id="72303-269">委派，可用來設定其他的 WebSocket 選項。</span><span class="sxs-lookup"><span data-stu-id="72303-269">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="72303-270">收到的執行個體[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) ，可用來設定選項。</span><span class="sxs-lookup"><span data-stu-id="72303-270">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="72303-271">以星號 （\*） 標示的選項還可在 JavaScript 用戶端，因為在瀏覽器 Api 中的限制中設定。</span><span class="sxs-lookup"><span data-stu-id="72303-271">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="72303-272">在.NET 用戶端，可以修改這些選項所提供的選項委派`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="72303-272">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="72303-273">在 JavaScript 用戶端，可以提供這些選項中提供的 JavaScript 物件`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72303-273">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="72303-274">其他資源</span><span class="sxs-lookup"><span data-stu-id="72303-274">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
