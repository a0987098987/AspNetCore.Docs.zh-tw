---
title: ASP.NET Core SignalR 設定
author: bradygaster
description: 瞭解如何設定 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 156ffac83fbdf61fd88ad8acc307c2c701c46bca
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773923"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="af249-103">ASP.NET Core SignalR 設定</span><span class="sxs-lookup"><span data-stu-id="af249-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="af249-104">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="af249-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="af249-105">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息：[JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="af249-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="af249-106">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="af249-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="af249-107">您可以在伺服器上使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法來設定 JSON 序列化，這可在您`Startup.ConfigureServices`的方法中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="af249-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="af249-108">方法會接受可`options`接收物件的委派。 `AddJsonProtocol`</span><span class="sxs-lookup"><span data-stu-id="af249-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="af249-109">該物件上的[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性是 JSON.NET `JsonSerializerSettings`物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="af249-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="af249-110">如需詳細資訊，請參閱[JSON.NET 檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="af249-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="af249-111">例如，若要設定序列化程式使用 "PascalCase" 屬性名稱，而不是預設的 "camelCase" 名稱，請使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="af249-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="af249-112">在 .net 用戶端中，相同`AddJsonProtocol`的擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="af249-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="af249-113">必須匯入命名空間，才能解析擴充方法： `Microsoft.Extensions.DependencyInjection`</span><span class="sxs-lookup"><span data-stu-id="af249-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="af249-114">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="af249-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="af249-115">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="af249-115">MessagePack serialization options</span></span>

<span data-ttu-id="af249-116">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="af249-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="af249-117">如需詳細資訊，請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="af249-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="af249-118">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="af249-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="af249-119">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="af249-119">Configure server options</span></span>

<span data-ttu-id="af249-120">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="af249-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="af249-121">選項</span><span class="sxs-lookup"><span data-stu-id="af249-121">Option</span></span> | <span data-ttu-id="af249-122">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-122">Default Value</span></span> | <span data-ttu-id="af249-123">說明</span><span class="sxs-lookup"><span data-stu-id="af249-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="af249-124">30 秒</span><span class="sxs-lookup"><span data-stu-id="af249-124">30 seconds</span></span> | <span data-ttu-id="af249-125">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="af249-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="af249-126">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="af249-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="af249-127">建議的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="af249-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="af249-128">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-128">15 seconds</span></span> | <span data-ttu-id="af249-129">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="af249-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="af249-130">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="af249-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="af249-131">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="af249-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="af249-132">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-132">15 seconds</span></span> | <span data-ttu-id="af249-133">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="af249-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="af249-134">在變更時`ServerTimeout` / `serverTimeoutInMilliseconds` ，請變更用戶端上的設定。 `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="af249-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="af249-135">建議`ServerTimeout` / `KeepAliveInterval`的值為值的雙精度浮點數。 `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="af249-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="af249-136">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="af249-136">All installed protocols</span></span> | <span data-ttu-id="af249-137">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="af249-137">Protocols supported by this hub.</span></span> <span data-ttu-id="af249-138">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="af249-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="af249-139">如果`true`為，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="af249-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="af249-140">預設值為`false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="af249-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="af249-141">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="af249-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="af249-142">若達到此限制，則在伺服器處理資料流程專案之前，會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="af249-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="af249-143">32 KB</span><span class="sxs-lookup"><span data-stu-id="af249-143">32 KB</span></span> | <span data-ttu-id="af249-144">單一傳入中樞訊息的大小上限。</span><span class="sxs-lookup"><span data-stu-id="af249-144">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="af249-145">選項</span><span class="sxs-lookup"><span data-stu-id="af249-145">Option</span></span> | <span data-ttu-id="af249-146">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-146">Default Value</span></span> | <span data-ttu-id="af249-147">描述</span><span class="sxs-lookup"><span data-stu-id="af249-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="af249-148">30 秒</span><span class="sxs-lookup"><span data-stu-id="af249-148">30 seconds</span></span> | <span data-ttu-id="af249-149">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="af249-149">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="af249-150">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="af249-150">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="af249-151">建議的值為值的`KeepAliveInterval`雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="af249-151">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="af249-152">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-152">15 seconds</span></span> | <span data-ttu-id="af249-153">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="af249-153">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="af249-154">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="af249-154">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="af249-155">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="af249-155">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="af249-156">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-156">15 seconds</span></span> | <span data-ttu-id="af249-157">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="af249-157">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="af249-158">在變更時`ServerTimeout` / `serverTimeoutInMilliseconds` ，請變更用戶端上的設定。 `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="af249-158">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="af249-159">建議`ServerTimeout` / `KeepAliveInterval`的值為值的雙精度浮點數。 `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="af249-159">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="af249-160">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="af249-160">All installed protocols</span></span> | <span data-ttu-id="af249-161">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="af249-161">Protocols supported by this hub.</span></span> <span data-ttu-id="af249-162">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="af249-162">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="af249-163">如果`true`為，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="af249-163">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="af249-164">預設值為`false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="af249-164">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="af249-165">選項</span><span class="sxs-lookup"><span data-stu-id="af249-165">Option</span></span> | <span data-ttu-id="af249-166">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-166">Default Value</span></span> | <span data-ttu-id="af249-167">描述</span><span class="sxs-lookup"><span data-stu-id="af249-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="af249-168">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-168">15 seconds</span></span> | <span data-ttu-id="af249-169">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="af249-169">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="af249-170">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="af249-170">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="af249-171">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="af249-171">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="af249-172">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-172">15 seconds</span></span> | <span data-ttu-id="af249-173">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="af249-173">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="af249-174">在變更時`ServerTimeout` / `serverTimeoutInMilliseconds` ，請變更用戶端上的設定。 `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="af249-174">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="af249-175">建議`ServerTimeout` / `KeepAliveInterval`的值為值的雙精度浮點數。 `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="af249-175">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="af249-176">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="af249-176">All installed protocols</span></span> | <span data-ttu-id="af249-177">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="af249-177">Protocols supported by this hub.</span></span> <span data-ttu-id="af249-178">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="af249-178">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="af249-179">如果`true`為，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="af249-179">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="af249-180">預設值為`false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="af249-180">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="af249-181">您可以為所有中樞設定選項，方法是提供選項委派給`AddSignalR`中`Startup.ConfigureServices`的呼叫。</span><span class="sxs-lookup"><span data-stu-id="af249-181">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="af249-182">單一中樞的選項會覆寫中提供的全域`AddSignalR`選項，並可使用<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>下列方式進行設定：</span><span class="sxs-lookup"><span data-stu-id="af249-182">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="af249-183">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="af249-183">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="af249-184">使用`HttpConnectionDispatcherOptions`來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="af249-184">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="af249-185">這些選項的設定方式是將委派傳遞至中`Startup.Configure`的[MapHub\<T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) 。</span><span class="sxs-lookup"><span data-stu-id="af249-185">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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
::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="af249-186">使用`HttpConnectionDispatcherOptions`來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="af249-186">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="af249-187">這些選項的設定方式是將委派傳遞至中`Startup.Configure`的[MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) 。</span><span class="sxs-lookup"><span data-stu-id="af249-187">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

::: moniker-end

<span data-ttu-id="af249-188">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="af249-188">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="af249-189">選項</span><span class="sxs-lookup"><span data-stu-id="af249-189">Option</span></span> | <span data-ttu-id="af249-190">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-190">Default Value</span></span> | <span data-ttu-id="af249-191">說明</span><span class="sxs-lookup"><span data-stu-id="af249-191">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="af249-192">32 KB</span><span class="sxs-lookup"><span data-stu-id="af249-192">32 KB</span></span> | <span data-ttu-id="af249-193">在套用背壓之前，伺服器會緩衝的用戶端接收的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="af249-193">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="af249-194">增加此值可讓伺服器更快速地接收較大的訊息，而不需套用背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="af249-194">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="af249-195">從套用至中樞類別`Authorize`的屬性自動收集的資料。</span><span class="sxs-lookup"><span data-stu-id="af249-195">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="af249-196">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="af249-196">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="af249-197">32 KB</span><span class="sxs-lookup"><span data-stu-id="af249-197">32 KB</span></span> | <span data-ttu-id="af249-198">在觀察到背壓之前，伺服器會緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="af249-198">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="af249-199">增加此值可讓伺服器更快速地緩衝較大的訊息，而不需等待背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="af249-199">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="af249-200">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="af249-200">All Transports are enabled.</span></span> | <span data-ttu-id="af249-201">`HttpTransportType`值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="af249-201">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="af249-202">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="af249-202">See below.</span></span> | <span data-ttu-id="af249-203">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="af249-203">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="af249-204">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="af249-204">See below.</span></span> | <span data-ttu-id="af249-205">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="af249-205">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="af249-206">選項</span><span class="sxs-lookup"><span data-stu-id="af249-206">Option</span></span> | <span data-ttu-id="af249-207">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-207">Default Value</span></span> | <span data-ttu-id="af249-208">說明</span><span class="sxs-lookup"><span data-stu-id="af249-208">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="af249-209">32 KB</span><span class="sxs-lookup"><span data-stu-id="af249-209">32 KB</span></span> | <span data-ttu-id="af249-210">從用戶端接收伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="af249-210">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="af249-211">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="af249-211">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="af249-212">從套用至中樞類別`Authorize`的屬性自動收集的資料。</span><span class="sxs-lookup"><span data-stu-id="af249-212">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="af249-213">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="af249-213">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="af249-214">32 KB</span><span class="sxs-lookup"><span data-stu-id="af249-214">32 KB</span></span> | <span data-ttu-id="af249-215">由伺服器所緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="af249-215">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="af249-216">增加這個值可讓伺服器傳送較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="af249-216">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="af249-217">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="af249-217">All Transports are enabled.</span></span> | <span data-ttu-id="af249-218">`HttpTransportType`值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="af249-218">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="af249-219">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="af249-219">See below.</span></span> | <span data-ttu-id="af249-220">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="af249-220">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="af249-221">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="af249-221">See below.</span></span> | <span data-ttu-id="af249-222">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="af249-222">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="af249-223">長輪詢傳輸具有其他選項，可以使用`LongPolling`屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="af249-223">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="af249-224">選項</span><span class="sxs-lookup"><span data-stu-id="af249-224">Option</span></span> | <span data-ttu-id="af249-225">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-225">Default Value</span></span> | <span data-ttu-id="af249-226">描述</span><span class="sxs-lookup"><span data-stu-id="af249-226">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="af249-227">90秒</span><span class="sxs-lookup"><span data-stu-id="af249-227">90 seconds</span></span> | <span data-ttu-id="af249-228">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="af249-228">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="af249-229">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="af249-229">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="af249-230">WebSocket 傳輸具有可使用`WebSockets`屬性來設定的其他選項：</span><span class="sxs-lookup"><span data-stu-id="af249-230">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="af249-231">選項</span><span class="sxs-lookup"><span data-stu-id="af249-231">Option</span></span> | <span data-ttu-id="af249-232">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-232">Default Value</span></span> | <span data-ttu-id="af249-233">說明</span><span class="sxs-lookup"><span data-stu-id="af249-233">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="af249-234">5 秒</span><span class="sxs-lookup"><span data-stu-id="af249-234">5 seconds</span></span> | <span data-ttu-id="af249-235">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="af249-235">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="af249-236">可以用來將`Sec-WebSocket-Protocol`標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="af249-236">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="af249-237">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="af249-237">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="af249-238">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="af249-238">Configure client options</span></span>

<span data-ttu-id="af249-239">您可以在`HubConnectionBuilder`類型上設定用戶端選項（可在 .net 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="af249-239">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="af249-240">它也可在 JAVA 用戶端中使用，但`HttpHubConnectionBuilder`子類別則包含產生器設定選項， `HubConnection`以及本身。</span><span class="sxs-lookup"><span data-stu-id="af249-240">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="af249-241">設定記錄</span><span class="sxs-lookup"><span data-stu-id="af249-241">Configure logging</span></span>

<span data-ttu-id="af249-242">使用`ConfigureLogging`方法，在 .net 用戶端中設定記錄。</span><span class="sxs-lookup"><span data-stu-id="af249-242">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="af249-243">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="af249-243">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="af249-244">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="af249-244">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="af249-245">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="af249-245">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="af249-246">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="af249-246">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="af249-247">例如，若要啟用主控台記錄，請安裝`Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="af249-247">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="af249-248">`AddConsole`呼叫擴充方法：</span><span class="sxs-lookup"><span data-stu-id="af249-248">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="af249-249">在 JavaScript 用戶端中，有`configureLogging`類似的方法存在。</span><span class="sxs-lookup"><span data-stu-id="af249-249">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="af249-250">`LogLevel`提供值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="af249-250">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="af249-251">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="af249-251">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="af249-252">您也可以`string`提供代表記錄層級名稱的值，而不是值。`LogLevel`</span><span class="sxs-lookup"><span data-stu-id="af249-252">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="af249-253">在您無法存取常數的`LogLevel`環境中設定 SignalR 記錄時，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="af249-253">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="af249-254">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="af249-254">The following table lists the available log levels.</span></span> <span data-ttu-id="af249-255">您所提供的值`configureLogging` ，用來設定將記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="af249-255">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="af249-256">記錄在此層級的訊息，**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="af249-256">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="af249-257">String</span><span class="sxs-lookup"><span data-stu-id="af249-257">String</span></span>                      | <span data-ttu-id="af249-258">LogLevel</span><span class="sxs-lookup"><span data-stu-id="af249-258">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="af249-259">`info`**或**`information`</span><span class="sxs-lookup"><span data-stu-id="af249-259">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="af249-260">`warn`**或**`warning`</span><span class="sxs-lookup"><span data-stu-id="af249-260">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="af249-261">若要完全停`signalR.LogLevel.None` `configureLogging`用記錄，請在方法中指定。</span><span class="sxs-lookup"><span data-stu-id="af249-261">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="af249-262">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="af249-262">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="af249-263">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="af249-263">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="af249-264">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="af249-264">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="af249-265">下列程式碼片段示範如何搭配 SignalR JAVA `java.util.logging`用戶端使用。</span><span class="sxs-lookup"><span data-stu-id="af249-265">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="af249-266">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="af249-266">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="af249-267">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="af249-267">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="af249-268">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="af249-268">Configure allowed transports</span></span>

<span data-ttu-id="af249-269">SignalR 所使用的傳輸可以在`WithUrl`呼叫中設定（`withUrl`以 JavaScript 為限）。</span><span class="sxs-lookup"><span data-stu-id="af249-269">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="af249-270">值的`HttpTransportType`位 or 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="af249-270">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="af249-271">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="af249-271">All transports are enabled by default.</span></span>

<span data-ttu-id="af249-272">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="af249-272">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="af249-273">在 JavaScript 用戶端中，會透過在提供給`transport` `withUrl`的選項物件上設定欄位，來設定傳輸：</span><span class="sxs-lookup"><span data-stu-id="af249-273">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="af249-274">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="af249-274">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="af249-275">在 JAVA 用戶端中，會使用`withTransport` `HttpHubConnectionBuilder`上的方法來選取傳輸。</span><span class="sxs-lookup"><span data-stu-id="af249-275">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="af249-276">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="af249-276">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="af249-277">SignalR JAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="af249-277">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="af249-278">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="af249-278">Configure bearer authentication</span></span>

<span data-ttu-id="af249-279">若要提供驗證資料以及 SignalR 要求，請使用`AccessTokenProvider`選項（`accessTokenFactory`在 JavaScript 中）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="af249-279">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="af249-280">在 .net 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用`Authorization`類型為的`Bearer`標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="af249-280">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="af249-281">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="af249-281">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="af249-282">在這些情況下，存取權杖會以查詢字串值`access_token`的形式提供。</span><span class="sxs-lookup"><span data-stu-id="af249-282">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="af249-283">在 .net 用戶端中， `AccessTokenProvider`可以使用中`WithUrl`的選項委派來指定選項：</span><span class="sxs-lookup"><span data-stu-id="af249-283">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="af249-284">在 JavaScript 用戶端中，存取權杖是藉由在`accessTokenFactory` `withUrl`的選項物件上設定欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="af249-284">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="af249-285">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="af249-285">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="af249-286">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串 >](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="af249-286">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="af249-287">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="af249-287">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="af249-288">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="af249-288">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="af249-289">設定 timeout 和 keep-alive 行為的其他選項可`HubConnection`用於物件本身：</span><span class="sxs-lookup"><span data-stu-id="af249-289">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="af249-290">.NET</span><span class="sxs-lookup"><span data-stu-id="af249-290">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="af249-291">選項</span><span class="sxs-lookup"><span data-stu-id="af249-291">Option</span></span> | <span data-ttu-id="af249-292">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-292">Default value</span></span> | <span data-ttu-id="af249-293">描述</span><span class="sxs-lookup"><span data-stu-id="af249-293">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="af249-294">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="af249-294">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="af249-295">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="af249-295">Timeout for server activity.</span></span> <span data-ttu-id="af249-296">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `Closed`並觸發`onclose`事件（在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="af249-296">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="af249-297">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="af249-297">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="af249-298">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="af249-298">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="af249-299">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-299">15 seconds</span></span> | <span data-ttu-id="af249-300">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="af249-300">Timeout for initial server handshake.</span></span> <span data-ttu-id="af249-301">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`Closed`事件（`onclose`在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="af249-301">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="af249-302">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="af249-302">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="af249-303">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="af249-303">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="af249-304">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-304">15 seconds</span></span> | <span data-ttu-id="af249-305">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="af249-305">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="af249-306">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="af249-306">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="af249-307">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="af249-307">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="af249-308">在 .net 用戶端中，timeout 值會指定`TimeSpan`為值。</span><span class="sxs-lookup"><span data-stu-id="af249-308">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="af249-309">JavaScript</span><span class="sxs-lookup"><span data-stu-id="af249-309">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="af249-310">選項</span><span class="sxs-lookup"><span data-stu-id="af249-310">Option</span></span> | <span data-ttu-id="af249-311">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-311">Default value</span></span> | <span data-ttu-id="af249-312">說明</span><span class="sxs-lookup"><span data-stu-id="af249-312">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="af249-313">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="af249-313">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="af249-314">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="af249-314">Timeout for server activity.</span></span> <span data-ttu-id="af249-315">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onclose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="af249-315">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="af249-316">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="af249-316">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="af249-317">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="af249-317">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="af249-318">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="af249-318">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="af249-319">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="af249-319">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="af249-320">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="af249-320">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="af249-321">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="af249-321">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="af249-322">Java</span><span class="sxs-lookup"><span data-stu-id="af249-322">Java</span></span>](#tab/java)

| <span data-ttu-id="af249-323">選項</span><span class="sxs-lookup"><span data-stu-id="af249-323">Option</span></span> | <span data-ttu-id="af249-324">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-324">Default value</span></span> | <span data-ttu-id="af249-325">描述</span><span class="sxs-lookup"><span data-stu-id="af249-325">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="af249-326">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="af249-326">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="af249-327">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="af249-327">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="af249-328">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="af249-328">Timeout for server activity.</span></span> <span data-ttu-id="af249-329">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onClose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="af249-329">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="af249-330">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="af249-330">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="af249-331">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="af249-331">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="af249-332">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-332">15 seconds</span></span> | <span data-ttu-id="af249-333">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="af249-333">Timeout for initial server handshake.</span></span> <span data-ttu-id="af249-334">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="af249-334">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="af249-335">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="af249-335">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="af249-336">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="af249-336">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="af249-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="af249-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="af249-338">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="af249-338">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="af249-339">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="af249-339">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="af249-340">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="af249-340">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="af249-341">如果用戶端未在伺服器上的`ClientTimeoutInterval`集合中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="af249-341">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="af249-342">.NET</span><span class="sxs-lookup"><span data-stu-id="af249-342">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="af249-343">選項</span><span class="sxs-lookup"><span data-stu-id="af249-343">Option</span></span> | <span data-ttu-id="af249-344">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-344">Default value</span></span> | <span data-ttu-id="af249-345">說明</span><span class="sxs-lookup"><span data-stu-id="af249-345">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="af249-346">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="af249-346">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="af249-347">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="af249-347">Timeout for server activity.</span></span> <span data-ttu-id="af249-348">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `Closed`並觸發`onclose`事件（在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="af249-348">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="af249-349">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="af249-349">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="af249-350">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="af249-350">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="af249-351">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-351">15 seconds</span></span> | <span data-ttu-id="af249-352">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="af249-352">Timeout for initial server handshake.</span></span> <span data-ttu-id="af249-353">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`Closed`事件（`onclose`在 JavaScript 中）。</span><span class="sxs-lookup"><span data-stu-id="af249-353">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="af249-354">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="af249-354">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="af249-355">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="af249-355">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="af249-356">在 .net 用戶端中，timeout 值會指定`TimeSpan`為值。</span><span class="sxs-lookup"><span data-stu-id="af249-356">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="af249-357">JavaScript</span><span class="sxs-lookup"><span data-stu-id="af249-357">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="af249-358">選項</span><span class="sxs-lookup"><span data-stu-id="af249-358">Option</span></span> | <span data-ttu-id="af249-359">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-359">Default value</span></span> | <span data-ttu-id="af249-360">描述</span><span class="sxs-lookup"><span data-stu-id="af249-360">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="af249-361">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="af249-361">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="af249-362">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="af249-362">Timeout for server activity.</span></span> <span data-ttu-id="af249-363">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onclose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="af249-363">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="af249-364">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="af249-364">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="af249-365">建議的值是至少兩個伺服器`KeepAliveInterval`值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="af249-365">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="af249-366">Java</span><span class="sxs-lookup"><span data-stu-id="af249-366">Java</span></span>](#tab/java)

| <span data-ttu-id="af249-367">選項</span><span class="sxs-lookup"><span data-stu-id="af249-367">Option</span></span> | <span data-ttu-id="af249-368">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-368">Default value</span></span> | <span data-ttu-id="af249-369">描述</span><span class="sxs-lookup"><span data-stu-id="af249-369">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="af249-370">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="af249-370">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="af249-371">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="af249-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="af249-372">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="af249-372">Timeout for server activity.</span></span> <span data-ttu-id="af249-373">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線， `onClose`並觸發事件。</span><span class="sxs-lookup"><span data-stu-id="af249-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="af249-374">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="af249-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="af249-375">建議的值是至少為伺服器`KeepAliveInterval`值的兩個數字，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="af249-375">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="af249-376">15 秒</span><span class="sxs-lookup"><span data-stu-id="af249-376">15 seconds</span></span> | <span data-ttu-id="af249-377">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="af249-377">Timeout for initial server handshake.</span></span> <span data-ttu-id="af249-378">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發`onClose`事件。</span><span class="sxs-lookup"><span data-stu-id="af249-378">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="af249-379">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="af249-379">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="af249-380">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="af249-380">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="af249-381">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="af249-381">Configure additional options</span></span>

<span data-ttu-id="af249-382">`WithUrl`您可以在上`withUrl` `HubConnectionBuilder`的（JavaScript）方法中， `HttpHubConnectionBuilder`或在 JAVA 用戶端中的各種設定 api 上設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="af249-382">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="af249-383">.NET</span><span class="sxs-lookup"><span data-stu-id="af249-383">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="af249-384">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="af249-384">.NET Option</span></span> |  <span data-ttu-id="af249-385">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-385">Default value</span></span> | <span data-ttu-id="af249-386">描述</span><span class="sxs-lookup"><span data-stu-id="af249-386">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="af249-387">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="af249-387">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="af249-388">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="af249-388">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="af249-389">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="af249-389">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="af249-390">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="af249-390">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="af249-391">Empty</span><span class="sxs-lookup"><span data-stu-id="af249-391">Empty</span></span> | <span data-ttu-id="af249-392">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="af249-392">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="af249-393">Empty</span><span class="sxs-lookup"><span data-stu-id="af249-393">Empty</span></span> | <span data-ttu-id="af249-394">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="af249-394">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="af249-395">Empty</span><span class="sxs-lookup"><span data-stu-id="af249-395">Empty</span></span> | <span data-ttu-id="af249-396">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="af249-396">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="af249-397">5 秒</span><span class="sxs-lookup"><span data-stu-id="af249-397">5 seconds</span></span> | <span data-ttu-id="af249-398">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="af249-398">WebSockets only.</span></span> <span data-ttu-id="af249-399">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="af249-399">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="af249-400">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="af249-400">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="af249-401">Empty</span><span class="sxs-lookup"><span data-stu-id="af249-401">Empty</span></span> | <span data-ttu-id="af249-402">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="af249-402">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="af249-403">委派，可以用來設定或取代用來傳送`HttpMessageHandler` HTTP 要求的。</span><span class="sxs-lookup"><span data-stu-id="af249-403">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="af249-404">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="af249-404">Not used for WebSocket connections.</span></span> <span data-ttu-id="af249-405">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="af249-405">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="af249-406">請修改該預設值的設定並加以傳回，或傳回新`HttpMessageHandler`的實例。</span><span class="sxs-lookup"><span data-stu-id="af249-406">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="af249-407">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="af249-407">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="af249-408">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="af249-408">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="af249-409">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="af249-409">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="af249-410">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="af249-410">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="af249-411">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="af249-411">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="af249-412">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="af249-412">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="af249-413">JavaScript</span><span class="sxs-lookup"><span data-stu-id="af249-413">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="af249-414">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="af249-414">JavaScript Option</span></span> | <span data-ttu-id="af249-415">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-415">Default Value</span></span> | <span data-ttu-id="af249-416">描述</span><span class="sxs-lookup"><span data-stu-id="af249-416">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="af249-417">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="af249-417">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="af249-418">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="af249-418">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="af249-419">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="af249-419">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="af249-420">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="af249-420">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="af249-421">Java</span><span class="sxs-lookup"><span data-stu-id="af249-421">Java</span></span>](#tab/java)

| <span data-ttu-id="af249-422">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="af249-422">Java Option</span></span> | <span data-ttu-id="af249-423">預設值</span><span class="sxs-lookup"><span data-stu-id="af249-423">Default Value</span></span> | <span data-ttu-id="af249-424">描述</span><span class="sxs-lookup"><span data-stu-id="af249-424">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="af249-425">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="af249-425">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="af249-426">將此設`true`為以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="af249-426">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="af249-427">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="af249-427">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="af249-428">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="af249-428">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="af249-429">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="af249-429">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="af249-430">Empty</span><span class="sxs-lookup"><span data-stu-id="af249-430">Empty</span></span> | <span data-ttu-id="af249-431">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="af249-431">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="af249-432">在 .NET 用戶端中，這些選項可以由提供給`WithUrl`的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="af249-432">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="af249-433">在 JavaScript 用戶端中，可以在提供給`withUrl`的 javascript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="af249-433">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="af249-434">在 JAVA 用戶端中，您可以使用所`HttpHubConnectionBuilder`傳回之上的方法來設定這些選項。`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="af249-434">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="af249-435">其他資源</span><span class="sxs-lookup"><span data-stu-id="af249-435">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
