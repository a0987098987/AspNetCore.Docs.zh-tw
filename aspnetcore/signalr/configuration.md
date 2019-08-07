---
title: ASP.NET Core SignalR 設定
author: bradygaster
description: 瞭解如何設定 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 4706a1e2774fa9f6fb40085da944e8a82476ef05
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820041"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="12ca5-103">ASP.NET Core SignalR 設定</span><span class="sxs-lookup"><span data-stu-id="12ca5-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="12ca5-104">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="12ca5-105">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息:[JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="12ca5-106">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="12ca5-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="12ca5-107">您可以在伺服器上使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法來設定 JSON 序列化, 這可在您`Startup.ConfigureServices`的方法中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="12ca5-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="12ca5-108">方法會接受可`options`接收物件的委派。 `AddJsonProtocol`</span><span class="sxs-lookup"><span data-stu-id="12ca5-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="12ca5-109">該物件上的[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性是 JSON.NET `JsonSerializerSettings`物件, 可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="12ca5-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="12ca5-110">如需詳細資訊, 請參閱[JSON.NET 檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="12ca5-111">例如, 若要設定序列化程式使用 "PascalCase" 屬性名稱, 而不是預設的 "camelCase" 名稱, 請使用下列程式碼:</span><span class="sxs-lookup"><span data-stu-id="12ca5-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="12ca5-112">在 .net 用戶端中, 相同`AddJsonProtocol`的擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="12ca5-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="12ca5-113">必須匯入命名空間, 才能解析擴充方法: `Microsoft.Extensions.DependencyInjection`</span><span class="sxs-lookup"><span data-stu-id="12ca5-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="12ca5-114">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="12ca5-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="12ca5-115">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-115">MessagePack serialization options</span></span>

<span data-ttu-id="12ca5-116">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="12ca5-117">如需詳細資訊, 請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="12ca5-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="12ca5-118">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="12ca5-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="12ca5-119">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-119">Configure server options</span></span>

<span data-ttu-id="12ca5-120">下表說明設定 SignalR 中樞的選項:</span><span class="sxs-lookup"><span data-stu-id="12ca5-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="12ca5-121">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-121">Option</span></span> | <span data-ttu-id="12ca5-122">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-122">Default Value</span></span> | <span data-ttu-id="12ca5-123">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="12ca5-124">30 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-124">30 seconds</span></span> | <span data-ttu-id="12ca5-125">如果用戶端未在此間隔內收到訊息 (包括 keep-alive), 伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="12ca5-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="12ca5-126">這可能需要比此逾時間隔更長的時間, 讓用戶端實際被標示為已中斷連線, 因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="12ca5-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="12ca5-127">建議的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="12ca5-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="12ca5-128">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-128">15 seconds</span></span> | <span data-ttu-id="12ca5-129">如果用戶端未在此時間間隔內傳送初始交握訊息, 連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="12ca5-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="12ca5-130">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時, 才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="12ca5-131">如需交握程式的詳細資訊, 請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="12ca5-132">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-132">15 seconds</span></span> | <span data-ttu-id="12ca5-133">如果伺服器未在此間隔內傳送訊息, 則會自動傳送 ping 訊息, 讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="12ca5-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="12ca5-134">在變更時`ServerTimeout` / `serverTimeoutInMilliseconds` , 請變更用戶端上的設定。 `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="12ca5-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="12ca5-135">建議`ServerTimeout` / `KeepAliveInterval`的值為值的雙精度浮點數。 `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="12ca5-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="12ca5-136">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="12ca5-136">All installed protocols</span></span> | <span data-ttu-id="12ca5-137">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-137">Protocols supported by this hub.</span></span> <span data-ttu-id="12ca5-138">根據預設, 允許在伺服器上註冊的所有通訊協定, 但是可以從這份清單中移除通訊協定, 以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="12ca5-139">如果`true`為, 當中樞方法擲回例外狀況時, 會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="12ca5-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="12ca5-140">預設值為`false`, 因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="12ca5-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="12ca5-141">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="12ca5-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="12ca5-142">若達到此限制, 則在伺服器處理資料流程專案之前, 會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="12ca5-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="12ca5-143">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-143">Option</span></span> | <span data-ttu-id="12ca5-144">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-144">Default Value</span></span> | <span data-ttu-id="12ca5-145">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-145">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="12ca5-146">30 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-146">30 seconds</span></span> | <span data-ttu-id="12ca5-147">如果用戶端未在此間隔內收到訊息 (包括 keep-alive), 伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="12ca5-147">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="12ca5-148">這可能需要比此逾時間隔更長的時間, 讓用戶端實際被標示為已中斷連線, 因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="12ca5-148">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="12ca5-149">建議的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="12ca5-149">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="12ca5-150">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-150">15 seconds</span></span> | <span data-ttu-id="12ca5-151">如果用戶端未在此時間間隔內傳送初始交握訊息, 連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="12ca5-151">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="12ca5-152">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時, 才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-152">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="12ca5-153">如需交握程式的詳細資訊, 請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-153">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="12ca5-154">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-154">15 seconds</span></span> | <span data-ttu-id="12ca5-155">如果伺服器未在此間隔內傳送訊息, 則會自動傳送 ping 訊息, 讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="12ca5-155">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="12ca5-156">在變更時`ServerTimeout` / `serverTimeoutInMilliseconds` , 請變更用戶端上的設定。 `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="12ca5-156">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="12ca5-157">建議`ServerTimeout` / `KeepAliveInterval`的值為值的雙精度浮點數。 `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="12ca5-157">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="12ca5-158">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="12ca5-158">All installed protocols</span></span> | <span data-ttu-id="12ca5-159">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-159">Protocols supported by this hub.</span></span> <span data-ttu-id="12ca5-160">根據預設, 允許在伺服器上註冊的所有通訊協定, 但是可以從這份清單中移除通訊協定, 以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-160">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="12ca5-161">如果`true`為, 當中樞方法擲回例外狀況時, 會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="12ca5-161">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="12ca5-162">預設值為`false`, 因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="12ca5-162">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="12ca5-163">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-163">Option</span></span> | <span data-ttu-id="12ca5-164">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-164">Default Value</span></span> | <span data-ttu-id="12ca5-165">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-165">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="12ca5-166">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-166">15 seconds</span></span> | <span data-ttu-id="12ca5-167">如果用戶端未在此時間間隔內傳送初始交握訊息, 連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="12ca5-167">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="12ca5-168">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時, 才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-168">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="12ca5-169">如需交握程式的詳細資訊, 請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-169">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="12ca5-170">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-170">15 seconds</span></span> | <span data-ttu-id="12ca5-171">如果伺服器未在此間隔內傳送訊息, 則會自動傳送 ping 訊息, 讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="12ca5-171">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="12ca5-172">在變更時`ServerTimeout` / `serverTimeoutInMilliseconds` , 請變更用戶端上的設定。 `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="12ca5-172">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="12ca5-173">建議`ServerTimeout` / `KeepAliveInterval`的值為值的雙精度浮點數。 `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="12ca5-173">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="12ca5-174">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="12ca5-174">All installed protocols</span></span> | <span data-ttu-id="12ca5-175">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-175">Protocols supported by this hub.</span></span> <span data-ttu-id="12ca5-176">根據預設, 允許在伺服器上註冊的所有通訊協定, 但是可以從這份清單中移除通訊協定, 以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-176">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="12ca5-177">如果`true`為, 當中樞方法擲回例外狀況時, 會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="12ca5-177">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="12ca5-178">預設值為`false`, 因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="12ca5-178">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="12ca5-179">您可以為所有中樞設定選項, 方法是提供選項委派給`AddSignalR`中`Startup.ConfigureServices`的呼叫。</span><span class="sxs-lookup"><span data-stu-id="12ca5-179">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="12ca5-180">單一中樞的選項會覆寫中提供的全域`AddSignalR`選項, 並可使用<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>下列方式進行設定:</span><span class="sxs-lookup"><span data-stu-id="12ca5-180">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="12ca5-181">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-181">Advanced HTTP configuration options</span></span>

<span data-ttu-id="12ca5-182">使用`HttpConnectionDispatcherOptions`來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-182">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="12ca5-183">這些選項的設定方式是將委派傳遞至中`Startup.Configure`的[MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) 。</span><span class="sxs-lookup"><span data-stu-id="12ca5-183">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="12ca5-184">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項:</span><span class="sxs-lookup"><span data-stu-id="12ca5-184">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="12ca5-185">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-185">Option</span></span> | <span data-ttu-id="12ca5-186">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-186">Default Value</span></span> | <span data-ttu-id="12ca5-187">說明</span><span class="sxs-lookup"><span data-stu-id="12ca5-187">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="12ca5-188">32 KB</span><span class="sxs-lookup"><span data-stu-id="12ca5-188">32 KB</span></span> | <span data-ttu-id="12ca5-189">從用戶端接收伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="12ca5-189">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="12ca5-190">增加這個值可讓伺服器接收較大的訊息, 但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="12ca5-190">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="12ca5-191">從套用至中樞類別`Authorize`的屬性自動收集的資料。</span><span class="sxs-lookup"><span data-stu-id="12ca5-191">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="12ca5-192">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="12ca5-192">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="12ca5-193">32 KB</span><span class="sxs-lookup"><span data-stu-id="12ca5-193">32 KB</span></span> | <span data-ttu-id="12ca5-194">由伺服器所緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="12ca5-194">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="12ca5-195">增加這個值可讓伺服器傳送較大的訊息, 但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="12ca5-195">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="12ca5-196">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="12ca5-196">All Transports are enabled.</span></span> | <span data-ttu-id="12ca5-197">`HttpTransportType`值的位元遮罩, 可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="12ca5-197">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="12ca5-198">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="12ca5-198">See below.</span></span> | <span data-ttu-id="12ca5-199">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="12ca5-199">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="12ca5-200">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="12ca5-200">See below.</span></span> | <span data-ttu-id="12ca5-201">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="12ca5-201">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="12ca5-202">長輪詢傳輸具有其他選項, 可以使用`LongPolling`屬性來設定:</span><span class="sxs-lookup"><span data-stu-id="12ca5-202">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="12ca5-203">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-203">Option</span></span> | <span data-ttu-id="12ca5-204">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-204">Default Value</span></span> | <span data-ttu-id="12ca5-205">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-205">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="12ca5-206">90秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-206">90 seconds</span></span> | <span data-ttu-id="12ca5-207">在終止單一輪詢要求之前, 伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="12ca5-207">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="12ca5-208">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="12ca5-208">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="12ca5-209">WebSocket 傳輸具有可使用`WebSockets`屬性來設定的其他選項:</span><span class="sxs-lookup"><span data-stu-id="12ca5-209">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="12ca5-210">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-210">Option</span></span> | <span data-ttu-id="12ca5-211">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-211">Default Value</span></span> | <span data-ttu-id="12ca5-212">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-212">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="12ca5-213">5 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-213">5 seconds</span></span> | <span data-ttu-id="12ca5-214">伺服器關閉之後, 如果用戶端無法在此時間間隔內關閉, 連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="12ca5-214">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="12ca5-215">可以用來將`Sec-WebSocket-Protocol`標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="12ca5-215">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="12ca5-216">委派會接收用戶端要求的值做為輸入, 而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="12ca5-216">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="12ca5-217">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-217">Configure client options</span></span>

<span data-ttu-id="12ca5-218">您可以在`HubConnectionBuilder`類型上設定用戶端選項 (可在 .net 和 JavaScript 用戶端中使用)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-218">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="12ca5-219">它也可在 JAVA 用戶端中使用, 但`HttpHubConnectionBuilder`子類別則包含產生器設定選項, `HubConnection`以及本身。</span><span class="sxs-lookup"><span data-stu-id="12ca5-219">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="12ca5-220">設定記錄</span><span class="sxs-lookup"><span data-stu-id="12ca5-220">Configure logging</span></span>

<span data-ttu-id="12ca5-221">使用`ConfigureLogging`方法, 在 .net 用戶端中設定記錄。</span><span class="sxs-lookup"><span data-stu-id="12ca5-221">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="12ca5-222">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="12ca5-222">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="12ca5-223">如需詳細資訊, 請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-223">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="12ca5-224">若要註冊記錄提供者, 您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="12ca5-224">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="12ca5-225">如需完整清單, 請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="12ca5-225">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="12ca5-226">例如, 若要啟用主控台記錄, 請安裝`Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="12ca5-226">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="12ca5-227">`AddConsole`呼叫擴充方法:</span><span class="sxs-lookup"><span data-stu-id="12ca5-227">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="12ca5-228">在 JavaScript 用戶端中, 有`configureLogging`類似的方法存在。</span><span class="sxs-lookup"><span data-stu-id="12ca5-228">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="12ca5-229">`LogLevel`提供值, 指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="12ca5-229">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="12ca5-230">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="12ca5-230">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="12ca5-231">您也可以`string`提供代表記錄層級名稱的值, 而不是值。`LogLevel`</span><span class="sxs-lookup"><span data-stu-id="12ca5-231">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="12ca5-232">在您無法存取常數的`LogLevel`環境中設定 SignalR 記錄時, 這會很有用。</span><span class="sxs-lookup"><span data-stu-id="12ca5-232">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="12ca5-233">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="12ca5-233">The following table lists the available log levels.</span></span> <span data-ttu-id="12ca5-234">您所提供的值`configureLogging` , 用來設定將記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="12ca5-234">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="12ca5-235">記錄在此層級的訊息,**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="12ca5-235">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="12ca5-236">String</span><span class="sxs-lookup"><span data-stu-id="12ca5-236">String</span></span>                      | <span data-ttu-id="12ca5-237">LogLevel</span><span class="sxs-lookup"><span data-stu-id="12ca5-237">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="12ca5-238">`info`**或**`information`</span><span class="sxs-lookup"><span data-stu-id="12ca5-238">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="12ca5-239">`warn`**或**`warning`</span><span class="sxs-lookup"><span data-stu-id="12ca5-239">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="12ca5-240">若要完全停`signalR.LogLevel.None` `configureLogging`用記錄, 請在方法中指定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-240">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="12ca5-241">如需記錄的詳細資訊, 請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-241">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="12ca5-242">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="12ca5-242">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="12ca5-243">它是高階記錄 API, 可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="12ca5-243">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="12ca5-244">下列程式碼片段示範如何搭配 SignalR JAVA `java.util.logging`用戶端使用。</span><span class="sxs-lookup"><span data-stu-id="12ca5-244">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="12ca5-245">如果您未在相依性中設定記錄, SLF4J 會載入預設的無作業記錄器, 並顯示下列警告訊息:</span><span class="sxs-lookup"><span data-stu-id="12ca5-245">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="12ca5-246">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="12ca5-246">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="12ca5-247">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="12ca5-247">Configure allowed transports</span></span>

<span data-ttu-id="12ca5-248">SignalR 所使用的傳輸可以在`WithUrl`呼叫中設定 (`withUrl`以 JavaScript 為限)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-248">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="12ca5-249">值的`HttpTransportType`位 or 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="12ca5-249">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="12ca5-250">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="12ca5-250">All transports are enabled by default.</span></span>

<span data-ttu-id="12ca5-251">例如, 若要停用伺服器傳送的事件傳輸, 但允許 Websocket 和較長的輪詢連接:</span><span class="sxs-lookup"><span data-stu-id="12ca5-251">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="12ca5-252">在 JavaScript 用戶端中, 會透過在提供給`transport` `withUrl`的選項物件上設定欄位, 來設定傳輸:</span><span class="sxs-lookup"><span data-stu-id="12ca5-252">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="12ca5-253">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="12ca5-253">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="12ca5-254">在 JAVA 用戶端中, 會使用`withTransport` `HttpHubConnectionBuilder`上的方法來選取傳輸。</span><span class="sxs-lookup"><span data-stu-id="12ca5-254">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="12ca5-255">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="12ca5-255">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="12ca5-256">SignalR JAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="12ca5-256">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="12ca5-257">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="12ca5-257">Configure bearer authentication</span></span>

<span data-ttu-id="12ca5-258">若要提供驗證資料以及 SignalR 要求, 請使用`AccessTokenProvider`選項 (`accessTokenFactory`在 JavaScript 中) 來指定函式, 以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="12ca5-258">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="12ca5-259">在 .net 用戶端中, 此存取權杖會當做 HTTP 「持有人驗證」權杖 (使用`Authorization`類型為的`Bearer`標頭) 傳入。</span><span class="sxs-lookup"><span data-stu-id="12ca5-259">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="12ca5-260">在 JavaScript 用戶端中, 存取權杖會用來做為持有人權杖,**但**在少數情況下, 瀏覽器 api 會限制套用標頭的功能 (特別是在伺服器傳送的事件和 websocket 要求中)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-260">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="12ca5-261">在這些情況下, 存取權杖會以查詢字串值`access_token`的形式提供。</span><span class="sxs-lookup"><span data-stu-id="12ca5-261">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="12ca5-262">在 .net 用戶端中, `AccessTokenProvider`可以使用中`WithUrl`的選項委派來指定選項:</span><span class="sxs-lookup"><span data-stu-id="12ca5-262">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="12ca5-263">在 JavaScript 用戶端中, 存取權杖是藉由在`accessTokenFactory` `withUrl`的選項物件上設定欄位來設定:</span><span class="sxs-lookup"><span data-stu-id="12ca5-263">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="12ca5-264">在 SignalR JAVA 用戶端中, 您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java), 設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="12ca5-264">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="12ca5-265">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串 >](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-265">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="12ca5-266">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), 您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="12ca5-266">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="12ca5-267">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-267">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="12ca5-268">設定 timeout 和 keep-alive 行為的其他選項可`HubConnection`用於物件本身:</span><span class="sxs-lookup"><span data-stu-id="12ca5-268">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="12ca5-269">.NET</span><span class="sxs-lookup"><span data-stu-id="12ca5-269">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="12ca5-270">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-270">Option</span></span> | <span data-ttu-id="12ca5-271">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-271">Default value</span></span> | <span data-ttu-id="12ca5-272">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-272">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="12ca5-273">30秒 (30000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="12ca5-273">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="12ca5-274">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="12ca5-274">Timeout for server activity.</span></span> <span data-ttu-id="12ca5-275">如果伺服器未在此間隔內傳送訊息, 用戶端會將伺服器視為已中斷連線, `Closed`並觸發`onclose`事件 (在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-275">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="12ca5-276">這個值必須夠大, 才能從伺服器傳送 ping 訊息 **, 並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="12ca5-276">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="12ca5-277">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位, 以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="12ca5-277">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="12ca5-278">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-278">15 seconds</span></span> | <span data-ttu-id="12ca5-279">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="12ca5-279">Timeout for initial server handshake.</span></span> <span data-ttu-id="12ca5-280">如果伺服器未在此間隔內傳送交握回應, 用戶端會取消交握並觸發`Closed`事件 (`onclose`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-280">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="12ca5-281">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時, 才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-281">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="12ca5-282">如需交握程式的詳細資訊, 請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-282">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="12ca5-283">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-283">15 seconds</span></span> | <span data-ttu-id="12ca5-284">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="12ca5-284">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="12ca5-285">從用戶端傳送任何訊息時, 會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="12ca5-285">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="12ca5-286">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息, 伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="12ca5-286">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="12ca5-287">在 .net 用戶端中, timeout 值會指定`TimeSpan`為值。</span><span class="sxs-lookup"><span data-stu-id="12ca5-287">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="12ca5-288">JavaScript</span><span class="sxs-lookup"><span data-stu-id="12ca5-288">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="12ca5-289">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-289">Option</span></span> | <span data-ttu-id="12ca5-290">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-290">Default value</span></span> | <span data-ttu-id="12ca5-291">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-291">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="12ca5-292">30秒 (30000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="12ca5-292">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="12ca5-293">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="12ca5-293">Timeout for server activity.</span></span> <span data-ttu-id="12ca5-294">如果伺服器未在此間隔內傳送訊息, 用戶端會將伺服器視為已中斷連線, `onclose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="12ca5-294">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="12ca5-295">這個值必須夠大, 才能從伺服器傳送 ping 訊息 **, 並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="12ca5-295">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="12ca5-296">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位, 以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="12ca5-296">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="12ca5-297">15秒 (15000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="12ca5-297">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="12ca5-298">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="12ca5-298">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="12ca5-299">從用戶端傳送任何訊息時, 會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="12ca5-299">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="12ca5-300">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息, 伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="12ca5-300">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="12ca5-301">Java</span><span class="sxs-lookup"><span data-stu-id="12ca5-301">Java</span></span>](#tab/java)

| <span data-ttu-id="12ca5-302">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-302">Option</span></span> | <span data-ttu-id="12ca5-303">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-303">Default value</span></span> | <span data-ttu-id="12ca5-304">說明</span><span class="sxs-lookup"><span data-stu-id="12ca5-304">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="12ca5-305">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="12ca5-305">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="12ca5-306">30秒 (30000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="12ca5-306">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="12ca5-307">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="12ca5-307">Timeout for server activity.</span></span> <span data-ttu-id="12ca5-308">如果伺服器未在此間隔內傳送訊息, 用戶端會將伺服器視為已中斷連線, `onClose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="12ca5-308">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="12ca5-309">這個值必須夠大, 才能從伺服器傳送 ping 訊息 **, 並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="12ca5-309">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="12ca5-310">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位, 以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="12ca5-310">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="12ca5-311">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-311">15 seconds</span></span> | <span data-ttu-id="12ca5-312">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="12ca5-312">Timeout for initial server handshake.</span></span> <span data-ttu-id="12ca5-313">如果伺服器未在此間隔內傳送交握回應, 用戶端會取消交握並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="12ca5-313">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="12ca5-314">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時, 才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-314">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="12ca5-315">如需交握程式的詳細資訊, 請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-315">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="12ca5-316">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="12ca5-316">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="12ca5-317">15秒 (15000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="12ca5-317">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="12ca5-318">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="12ca5-318">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="12ca5-319">從用戶端傳送任何訊息時, 會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="12ca5-319">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="12ca5-320">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息, 伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="12ca5-320">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="12ca5-321">.NET</span><span class="sxs-lookup"><span data-stu-id="12ca5-321">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="12ca5-322">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-322">Option</span></span> | <span data-ttu-id="12ca5-323">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-323">Default value</span></span> | <span data-ttu-id="12ca5-324">說明</span><span class="sxs-lookup"><span data-stu-id="12ca5-324">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="12ca5-325">30秒 (30000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="12ca5-325">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="12ca5-326">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="12ca5-326">Timeout for server activity.</span></span> <span data-ttu-id="12ca5-327">如果伺服器未在此間隔內傳送訊息, 用戶端會將伺服器視為已中斷連線, `Closed`並觸發`onclose`事件 (在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-327">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="12ca5-328">這個值必須夠大, 才能從伺服器傳送 ping 訊息 **, 並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="12ca5-328">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="12ca5-329">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位, 以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="12ca5-329">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="12ca5-330">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-330">15 seconds</span></span> | <span data-ttu-id="12ca5-331">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="12ca5-331">Timeout for initial server handshake.</span></span> <span data-ttu-id="12ca5-332">如果伺服器未在此間隔內傳送交握回應, 用戶端會取消交握並觸發`Closed`事件 (`onclose`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-332">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="12ca5-333">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時, 才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-333">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="12ca5-334">如需交握程式的詳細資訊, 請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-334">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="12ca5-335">在 .net 用戶端中, timeout 值會指定`TimeSpan`為值。</span><span class="sxs-lookup"><span data-stu-id="12ca5-335">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="12ca5-336">JavaScript</span><span class="sxs-lookup"><span data-stu-id="12ca5-336">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="12ca5-337">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-337">Option</span></span> | <span data-ttu-id="12ca5-338">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-338">Default value</span></span> | <span data-ttu-id="12ca5-339">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-339">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="12ca5-340">30秒 (30000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="12ca5-340">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="12ca5-341">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="12ca5-341">Timeout for server activity.</span></span> <span data-ttu-id="12ca5-342">如果伺服器未在此間隔內傳送訊息, 用戶端會將伺服器視為已中斷連線, `onclose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="12ca5-342">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="12ca5-343">這個值必須夠大, 才能從伺服器傳送 ping 訊息 **, 並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="12ca5-343">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="12ca5-344">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位, 以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="12ca5-344">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="12ca5-345">Java</span><span class="sxs-lookup"><span data-stu-id="12ca5-345">Java</span></span>](#tab/java)

| <span data-ttu-id="12ca5-346">選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-346">Option</span></span> | <span data-ttu-id="12ca5-347">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-347">Default value</span></span> | <span data-ttu-id="12ca5-348">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-348">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="12ca5-349">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="12ca5-349">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="12ca5-350">30秒 (30000 毫秒)</span><span class="sxs-lookup"><span data-stu-id="12ca5-350">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="12ca5-351">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="12ca5-351">Timeout for server activity.</span></span> <span data-ttu-id="12ca5-352">如果伺服器未在此間隔內傳送訊息, 用戶端會將伺服器視為已中斷連線, `onClose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="12ca5-352">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="12ca5-353">這個值必須夠大, 才能從伺服器傳送 ping 訊息 **, 並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="12ca5-353">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="12ca5-354">建議的值是至少為伺服器`KeepAliveInterval`值的兩個數字, 以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="12ca5-354">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="12ca5-355">15 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-355">15 seconds</span></span> | <span data-ttu-id="12ca5-356">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="12ca5-356">Timeout for initial server handshake.</span></span> <span data-ttu-id="12ca5-357">如果伺服器未在此間隔內傳送交握回應, 用戶端會取消交握並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="12ca5-357">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="12ca5-358">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時, 才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-358">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="12ca5-359">如需交握程式的詳細資訊, 請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="12ca5-359">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="12ca5-360">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-360">Configure additional options</span></span>

<span data-ttu-id="12ca5-361">`WithUrl`您可以在上`withUrl` `HubConnectionBuilder`的 (JavaScript) 方法中, `HttpHubConnectionBuilder`或在 JAVA 用戶端中的各種設定 api 上設定其他選項:</span><span class="sxs-lookup"><span data-stu-id="12ca5-361">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="12ca5-362">.NET</span><span class="sxs-lookup"><span data-stu-id="12ca5-362">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="12ca5-363">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-363">.NET Option</span></span> |  <span data-ttu-id="12ca5-364">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-364">Default value</span></span> | <span data-ttu-id="12ca5-365">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-365">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="12ca5-366">傳回字串的函式, 在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="12ca5-366">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="12ca5-367">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="12ca5-367">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="12ca5-368">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="12ca5-368">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="12ca5-369">使用 Azure SignalR Service 時, 無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-369">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="12ca5-370">Empty</span><span class="sxs-lookup"><span data-stu-id="12ca5-370">Empty</span></span> | <span data-ttu-id="12ca5-371">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="12ca5-371">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="12ca5-372">Empty</span><span class="sxs-lookup"><span data-stu-id="12ca5-372">Empty</span></span> | <span data-ttu-id="12ca5-373">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="12ca5-373">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="12ca5-374">Empty</span><span class="sxs-lookup"><span data-stu-id="12ca5-374">Empty</span></span> | <span data-ttu-id="12ca5-375">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="12ca5-375">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="12ca5-376">5 秒</span><span class="sxs-lookup"><span data-stu-id="12ca5-376">5 seconds</span></span> | <span data-ttu-id="12ca5-377">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="12ca5-377">WebSockets only.</span></span> <span data-ttu-id="12ca5-378">關閉伺服器以認可關閉要求之後, 用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="12ca5-378">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="12ca5-379">如果伺服器在這段時間內未認可關閉, 用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="12ca5-379">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="12ca5-380">Empty</span><span class="sxs-lookup"><span data-stu-id="12ca5-380">Empty</span></span> | <span data-ttu-id="12ca5-381">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="12ca5-381">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="12ca5-382">委派, 可以用來設定或取代用來傳送`HttpMessageHandler` HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="12ca5-382">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="12ca5-383">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="12ca5-383">Not used for WebSocket connections.</span></span> <span data-ttu-id="12ca5-384">這個委派必須傳回非 null 值, 而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="12ca5-384">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="12ca5-385">請修改該預設值的設定並加以傳回, 或傳回新`HttpMessageHandler`的實例。</span><span class="sxs-lookup"><span data-stu-id="12ca5-385">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="12ca5-386">**取代處理常式時, 請務必從提供的處理常式中複製您想要保留的設定, 否則設定的選項 (例如 Cookie 和標頭) 將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="12ca5-386">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="12ca5-387">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="12ca5-387">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="12ca5-388">設定此布林值, 以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="12ca5-388">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="12ca5-389">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="12ca5-389">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="12ca5-390">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="12ca5-390">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="12ca5-391">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="12ca5-391">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="12ca5-392">JavaScript</span><span class="sxs-lookup"><span data-stu-id="12ca5-392">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="12ca5-393">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-393">JavaScript Option</span></span> | <span data-ttu-id="12ca5-394">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-394">Default Value</span></span> | <span data-ttu-id="12ca5-395">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-395">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="12ca5-396">傳回字串的函式, 在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="12ca5-396">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="12ca5-397">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="12ca5-397">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="12ca5-398">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="12ca5-398">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="12ca5-399">使用 Azure SignalR Service 時, 無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-399">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="12ca5-400">Java</span><span class="sxs-lookup"><span data-stu-id="12ca5-400">Java</span></span>](#tab/java)

| <span data-ttu-id="12ca5-401">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="12ca5-401">Java Option</span></span> | <span data-ttu-id="12ca5-402">預設值</span><span class="sxs-lookup"><span data-stu-id="12ca5-402">Default Value</span></span> | <span data-ttu-id="12ca5-403">描述</span><span class="sxs-lookup"><span data-stu-id="12ca5-403">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="12ca5-404">傳回字串的函式, 在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="12ca5-404">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="12ca5-405">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="12ca5-405">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="12ca5-406">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="12ca5-406">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="12ca5-407">使用 Azure SignalR Service 時, 無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="12ca5-407">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="12ca5-408">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="12ca5-408">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="12ca5-409">Empty</span><span class="sxs-lookup"><span data-stu-id="12ca5-409">Empty</span></span> | <span data-ttu-id="12ca5-410">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="12ca5-410">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="12ca5-411">在 .NET 用戶端中, 這些選項可以由提供給`WithUrl`的選項委派進行修改:</span><span class="sxs-lookup"><span data-stu-id="12ca5-411">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="12ca5-412">在 JavaScript 用戶端中, 可以在提供給`withUrl`的 javascript 物件中提供這些選項:</span><span class="sxs-lookup"><span data-stu-id="12ca5-412">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="12ca5-413">在 JAVA 用戶端中, 您可以使用所`HttpHubConnectionBuilder`傳回之上的方法來設定這些選項。`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="12ca5-413">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="12ca5-414">其他資源</span><span class="sxs-lookup"><span data-stu-id="12ca5-414">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
