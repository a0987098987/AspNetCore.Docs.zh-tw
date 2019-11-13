---
title: ASP.NET Core SignalR 設定
author: bradygaster
description: 瞭解如何設定 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: 682cc36216a4dc9b38c87b4f67100ab565a70e5c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963975"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a><span data-ttu-id="ac06f-103">ASP.NET Core SignalR 設定</span><span class="sxs-lookup"><span data-stu-id="ac06f-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="ac06f-104">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="ac06f-105">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="ac06f-106">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="ac06f-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac06f-107">您可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，在伺服器上設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="ac06f-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="ac06f-108">`AddJsonProtocol` 可以在 `Startup.ConfigureServices`中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="ac06f-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ac06f-109">`AddJsonProtocol` 方法會接受一個可接收 `options` 物件的委派。</span><span class="sxs-lookup"><span data-stu-id="ac06f-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ac06f-110">該物件上的[PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是 `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> 物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="ac06f-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ac06f-111">如需詳細資訊，請參閱[system.web 檔](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="ac06f-112">例如，若要設定序列化程式不變更屬性名稱的大小寫，而不是預設的 "camelCase" 名稱，請在 `Startup.ConfigureServices`中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac06f-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="ac06f-113">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="ac06f-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ac06f-114">必須匯入 `Microsoft.Extensions.DependencyInjection` 命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="ac06f-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="ac06f-115">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="ac06f-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="ac06f-116">切換至 Newtonsoft. Json</span><span class="sxs-lookup"><span data-stu-id="ac06f-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="ac06f-117">如果您需要 `System.Text.Json`中不支援的 `Newtonsoft.Json` 功能，請參閱[切換至 Newtonsoft。](xref:migration/22-to-30#switch-to-newtonsoftjson)</span><span class="sxs-lookup"><span data-stu-id="ac06f-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="ac06f-118">您可以在伺服器上使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法來設定 JSON 序列化，這可在您的 `Startup.ConfigureServices` 方法中[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="ac06f-118">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ac06f-119">`AddJsonProtocol` 方法會接受一個可接收 `options` 物件的委派。</span><span class="sxs-lookup"><span data-stu-id="ac06f-119">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ac06f-120">該物件上的[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性是 JSON.NET `JsonSerializerSettings` 物件，可用於設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="ac06f-120">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ac06f-121">如需詳細資訊，請參閱[JSON.NET 檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-121">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="ac06f-122">例如，若要設定序列化程式使用 "PascalCase" 屬性名稱，而不是預設的 "camelCase" 名稱，請在 `Startup.ConfigureServices`中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac06f-122">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="ac06f-123">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="ac06f-123">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ac06f-124">必須匯入 `Microsoft.Extensions.DependencyInjection` 命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="ac06f-124">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="ac06f-125">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="ac06f-125">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

::: moniker-end

### <a name="messagepack-serialization-options"></a><span data-ttu-id="ac06f-126">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-126">MessagePack serialization options</span></span>

<span data-ttu-id="ac06f-127">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-127">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="ac06f-128">如需詳細資訊，請參閱[SignalR中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="ac06f-128">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="ac06f-129">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="ac06f-129">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="ac06f-130">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-130">Configure server options</span></span>

<span data-ttu-id="ac06f-131">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="ac06f-131">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="ac06f-132">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-132">Option</span></span> | <span data-ttu-id="ac06f-133">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-133">Default Value</span></span> | <span data-ttu-id="ac06f-134">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-134">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ac06f-135">30 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-135">30 seconds</span></span> | <span data-ttu-id="ac06f-136">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="ac06f-136">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ac06f-137">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="ac06f-137">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ac06f-138">建議的值是 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="ac06f-138">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ac06f-139">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-139">15 seconds</span></span> | <span data-ttu-id="ac06f-140">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="ac06f-140">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ac06f-141">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-141">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ac06f-142">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-142">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ac06f-143">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-143">15 seconds</span></span> | <span data-ttu-id="ac06f-144">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="ac06f-144">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ac06f-145">當變更 `KeepAliveInterval`時，請變更用戶端上的 `ServerTimeout`/`serverTimeoutInMilliseconds` 設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-145">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ac06f-146">建議的 `ServerTimeout`/`serverTimeoutInMilliseconds` 值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="ac06f-146">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ac06f-147">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="ac06f-147">All installed protocols</span></span> | <span data-ttu-id="ac06f-148">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-148">Protocols supported by this hub.</span></span> <span data-ttu-id="ac06f-149">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-149">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ac06f-150">如果 `true`，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac06f-150">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ac06f-151">預設值為 `false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="ac06f-151">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="ac06f-152">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="ac06f-152">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="ac06f-153">若達到此限制，則在伺服器處理資料流程專案之前，會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="ac06f-153">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="ac06f-154">32 KB</span><span class="sxs-lookup"><span data-stu-id="ac06f-154">32 KB</span></span> | <span data-ttu-id="ac06f-155">單一傳入中樞訊息的大小上限。</span><span class="sxs-lookup"><span data-stu-id="ac06f-155">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="ac06f-156">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-156">Option</span></span> | <span data-ttu-id="ac06f-157">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-157">Default Value</span></span> | <span data-ttu-id="ac06f-158">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-158">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ac06f-159">30 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-159">30 seconds</span></span> | <span data-ttu-id="ac06f-160">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="ac06f-160">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ac06f-161">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="ac06f-161">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ac06f-162">建議的值是 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="ac06f-162">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ac06f-163">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-163">15 seconds</span></span> | <span data-ttu-id="ac06f-164">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="ac06f-164">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ac06f-165">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-165">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ac06f-166">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-166">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ac06f-167">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-167">15 seconds</span></span> | <span data-ttu-id="ac06f-168">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="ac06f-168">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ac06f-169">當變更 `KeepAliveInterval`時，請變更用戶端上的 `ServerTimeout`/`serverTimeoutInMilliseconds` 設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-169">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ac06f-170">建議的 `ServerTimeout`/`serverTimeoutInMilliseconds` 值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="ac06f-170">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ac06f-171">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="ac06f-171">All installed protocols</span></span> | <span data-ttu-id="ac06f-172">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-172">Protocols supported by this hub.</span></span> <span data-ttu-id="ac06f-173">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-173">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ac06f-174">如果 `true`，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac06f-174">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ac06f-175">預設值為 `false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="ac06f-175">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="ac06f-176">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-176">Option</span></span> | <span data-ttu-id="ac06f-177">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-177">Default Value</span></span> | <span data-ttu-id="ac06f-178">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-178">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="ac06f-179">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-179">15 seconds</span></span> | <span data-ttu-id="ac06f-180">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="ac06f-180">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ac06f-181">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-181">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ac06f-182">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-182">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ac06f-183">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-183">15 seconds</span></span> | <span data-ttu-id="ac06f-184">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="ac06f-184">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ac06f-185">當變更 `KeepAliveInterval`時，請變更用戶端上的 `ServerTimeout`/`serverTimeoutInMilliseconds` 設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-185">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ac06f-186">建議的 `ServerTimeout`/`serverTimeoutInMilliseconds` 值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="ac06f-186">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ac06f-187">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="ac06f-187">All installed protocols</span></span> | <span data-ttu-id="ac06f-188">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-188">Protocols supported by this hub.</span></span> <span data-ttu-id="ac06f-189">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-189">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ac06f-190">如果 `true`，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac06f-190">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ac06f-191">預設值為 `false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="ac06f-191">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="ac06f-192">您可以為所有中樞設定選項，方法是在 `Startup.ConfigureServices`中提供 `AddSignalR` 呼叫的選項委派。</span><span class="sxs-lookup"><span data-stu-id="ac06f-192">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="ac06f-193">單一中樞的選項會覆寫 `AddSignalR` 中提供的全域選項，而且可以使用 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>來設定：</span><span class="sxs-lookup"><span data-stu-id="ac06f-193">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="ac06f-194">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-194">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac06f-195">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-195">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ac06f-196">這些選項是藉由將委派傳遞至 `Startup.Configure`中的[MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub)來設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-196">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="ac06f-197">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-197">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ac06f-198">這些選項是藉由將委派傳遞至 `Startup.Configure`中的[MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)來設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-198">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="ac06f-199">下表說明設定 ASP.NET Core SignalR的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="ac06f-199">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="ac06f-200">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-200">Option</span></span> | <span data-ttu-id="ac06f-201">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-201">Default Value</span></span> | <span data-ttu-id="ac06f-202">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-202">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ac06f-203">32 KB</span><span class="sxs-lookup"><span data-stu-id="ac06f-203">32 KB</span></span> | <span data-ttu-id="ac06f-204">在套用背壓之前，伺服器會緩衝的用戶端接收的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="ac06f-204">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="ac06f-205">增加此值可讓伺服器更快速地接收較大的訊息，而不需套用背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="ac06f-205">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ac06f-206">自動從套用至中樞類別的 `Authorize` 屬性收集資料。</span><span class="sxs-lookup"><span data-stu-id="ac06f-206">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ac06f-207">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="ac06f-207">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ac06f-208">32 KB</span><span class="sxs-lookup"><span data-stu-id="ac06f-208">32 KB</span></span> | <span data-ttu-id="ac06f-209">在觀察到背壓之前，伺服器會緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="ac06f-209">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="ac06f-210">增加此值可讓伺服器更快速地緩衝較大的訊息，而不需等待背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="ac06f-210">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ac06f-211">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="ac06f-211">All Transports are enabled.</span></span> | <span data-ttu-id="ac06f-212">`HttpTransportType` 值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="ac06f-212">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ac06f-213">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="ac06f-213">See below.</span></span> | <span data-ttu-id="ac06f-214">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="ac06f-214">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ac06f-215">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="ac06f-215">See below.</span></span> | <span data-ttu-id="ac06f-216">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="ac06f-216">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="ac06f-217">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-217">Option</span></span> | <span data-ttu-id="ac06f-218">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-218">Default Value</span></span> | <span data-ttu-id="ac06f-219">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-219">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ac06f-220">32 KB</span><span class="sxs-lookup"><span data-stu-id="ac06f-220">32 KB</span></span> | <span data-ttu-id="ac06f-221">從用戶端接收伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="ac06f-221">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="ac06f-222">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="ac06f-222">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ac06f-223">自動從套用至中樞類別的 `Authorize` 屬性收集資料。</span><span class="sxs-lookup"><span data-stu-id="ac06f-223">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ac06f-224">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="ac06f-224">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ac06f-225">32 KB</span><span class="sxs-lookup"><span data-stu-id="ac06f-225">32 KB</span></span> | <span data-ttu-id="ac06f-226">由伺服器所緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="ac06f-226">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="ac06f-227">增加這個值可讓伺服器傳送較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="ac06f-227">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ac06f-228">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="ac06f-228">All Transports are enabled.</span></span> | <span data-ttu-id="ac06f-229">`HttpTransportType` 值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="ac06f-229">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ac06f-230">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="ac06f-230">See below.</span></span> | <span data-ttu-id="ac06f-231">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="ac06f-231">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ac06f-232">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="ac06f-232">See below.</span></span> | <span data-ttu-id="ac06f-233">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="ac06f-233">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="ac06f-234">長輪詢傳輸具有其他選項，可以使用 `LongPolling` 屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="ac06f-234">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="ac06f-235">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-235">Option</span></span> | <span data-ttu-id="ac06f-236">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-236">Default Value</span></span> | <span data-ttu-id="ac06f-237">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-237">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="ac06f-238">90秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-238">90 seconds</span></span> | <span data-ttu-id="ac06f-239">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="ac06f-239">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="ac06f-240">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="ac06f-240">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="ac06f-241">WebSocket 傳輸具有其他選項，可以使用 `WebSockets` 屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="ac06f-241">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="ac06f-242">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-242">Option</span></span> | <span data-ttu-id="ac06f-243">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-243">Default Value</span></span> | <span data-ttu-id="ac06f-244">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-244">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="ac06f-245">5 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-245">5 seconds</span></span> | <span data-ttu-id="ac06f-246">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="ac06f-246">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="ac06f-247">可以用來將 `Sec-WebSocket-Protocol` 標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="ac06f-247">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="ac06f-248">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="ac06f-248">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="ac06f-249">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-249">Configure client options</span></span>

<span data-ttu-id="ac06f-250">您可以在 `HubConnectionBuilder` 類型上設定用戶端選項（可在 .NET 和 JavaScript 用戶端中使用）。</span><span class="sxs-lookup"><span data-stu-id="ac06f-250">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="ac06f-251">它也可在 JAVA 用戶端中使用，但 `HttpHubConnectionBuilder` 子類別則包含 builder 設定選項，以及 `HubConnection` 本身。</span><span class="sxs-lookup"><span data-stu-id="ac06f-251">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="ac06f-252">設定記錄</span><span class="sxs-lookup"><span data-stu-id="ac06f-252">Configure logging</span></span>

<span data-ttu-id="ac06f-253">記錄是在 .NET 用戶端中使用 `ConfigureLogging` 方法來設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-253">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="ac06f-254">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="ac06f-254">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="ac06f-255">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-255">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="ac06f-256">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="ac06f-256">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="ac06f-257">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="ac06f-257">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="ac06f-258">例如，若要啟用主控台記錄，請安裝 `Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="ac06f-258">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="ac06f-259">呼叫 `AddConsole` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="ac06f-259">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="ac06f-260">在 JavaScript 用戶端中，有類似的 `configureLogging` 方法存在。</span><span class="sxs-lookup"><span data-stu-id="ac06f-260">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="ac06f-261">提供 `LogLevel` 值，指出要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="ac06f-261">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="ac06f-262">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="ac06f-262">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac06f-263">您也可以提供代表記錄層級名稱的 `string` 值，而不是 `LogLevel` 值。</span><span class="sxs-lookup"><span data-stu-id="ac06f-263">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="ac06f-264">在您無法存取 `LogLevel` 常數的環境中設定 SignalR 記錄時，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="ac06f-264">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="ac06f-265">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="ac06f-265">The following table lists the available log levels.</span></span> <span data-ttu-id="ac06f-266">您提供給 `configureLogging` 的值會設定要記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="ac06f-266">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="ac06f-267">記錄在此層級的訊息，**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="ac06f-267">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="ac06f-268">String</span><span class="sxs-lookup"><span data-stu-id="ac06f-268">String</span></span>                      | <span data-ttu-id="ac06f-269">LogLevel</span><span class="sxs-lookup"><span data-stu-id="ac06f-269">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="ac06f-270">`info`**或**`information`</span><span class="sxs-lookup"><span data-stu-id="ac06f-270">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="ac06f-271">`warn`**或**`warning`</span><span class="sxs-lookup"><span data-stu-id="ac06f-271">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ac06f-272">若要完全停用記錄，請在 `configureLogging` 方法中指定 `signalR.LogLevel.None`。</span><span class="sxs-lookup"><span data-stu-id="ac06f-272">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ac06f-273">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-273">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="ac06f-274">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="ac06f-274">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="ac06f-275">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="ac06f-275">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="ac06f-276">下列程式碼片段示範如何使用 `java.util.logging` 搭配 SignalR JAVA 用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac06f-276">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="ac06f-277">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="ac06f-277">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="ac06f-278">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="ac06f-278">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="ac06f-279">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="ac06f-279">Configure allowed transports</span></span>

<span data-ttu-id="ac06f-280">SignalR 所使用的傳輸可以在 `WithUrl` 呼叫（在 JavaScript 中`withUrl`）中設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-280">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="ac06f-281">`HttpTransportType` 值的位 OR 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="ac06f-281">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="ac06f-282">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="ac06f-282">All transports are enabled by default.</span></span>

<span data-ttu-id="ac06f-283">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="ac06f-283">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="ac06f-284">在 JavaScript 用戶端中，藉由在提供給 `withUrl`的選項物件上設定 [`transport`] 欄位來設定傳輸：</span><span class="sxs-lookup"><span data-stu-id="ac06f-284">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ac06f-285">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="ac06f-285">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="ac06f-286">在 JAVA 用戶端中，會使用 `HttpHubConnectionBuilder`上的 `withTransport` 方法來選取傳輸。</span><span class="sxs-lookup"><span data-stu-id="ac06f-286">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="ac06f-287">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="ac06f-287">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="ac06f-288">SignalR 的 JAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="ac06f-288">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="ac06f-289">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="ac06f-289">Configure bearer authentication</span></span>

<span data-ttu-id="ac06f-290">若要提供驗證資料和 SignalR 要求，請使用 `AccessTokenProvider` 選項（JavaScript 中的`accessTokenFactory`）來指定會傳回所需存取權杖的函式。</span><span class="sxs-lookup"><span data-stu-id="ac06f-290">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="ac06f-291">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用具有 `Bearer`類型的 `Authorization` 標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="ac06f-291">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="ac06f-292">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="ac06f-292">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="ac06f-293">在這些情況下，存取權杖會當做查詢字串值提供 `access_token`。</span><span class="sxs-lookup"><span data-stu-id="ac06f-293">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="ac06f-294">在 .NET 用戶端中，可以使用 `WithUrl`中的選項委派來指定 `AccessTokenProvider` 選項：</span><span class="sxs-lookup"><span data-stu-id="ac06f-294">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="ac06f-295">在 JavaScript 用戶端中，存取權杖是藉由在 `withUrl`中的 options 物件上設定 [`accessTokenFactory`] 欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="ac06f-295">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="ac06f-296">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="ac06f-296">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="ac06f-297">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一\<字串 >](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-297">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="ac06f-298">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="ac06f-298">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="ac06f-299">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-299">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="ac06f-300">設定 timeout 和 keep-alive 行為的其他選項可用於 `HubConnection` 物件本身：</span><span class="sxs-lookup"><span data-stu-id="ac06f-300">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="ac06f-301">.NET</span><span class="sxs-lookup"><span data-stu-id="ac06f-301">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ac06f-302">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-302">Option</span></span> | <span data-ttu-id="ac06f-303">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-303">Default value</span></span> | <span data-ttu-id="ac06f-304">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-304">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ac06f-305">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="ac06f-305">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ac06f-306">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="ac06f-306">Timeout for server activity.</span></span> <span data-ttu-id="ac06f-307">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（在 JavaScript 中`onclose`）。</span><span class="sxs-lookup"><span data-stu-id="ac06f-307">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ac06f-308">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac06f-308">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ac06f-309">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="ac06f-309">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ac06f-310">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-310">15 seconds</span></span> | <span data-ttu-id="ac06f-311">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="ac06f-311">Timeout for initial server handshake.</span></span> <span data-ttu-id="ac06f-312">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（在 JavaScript 中`onclose`）。</span><span class="sxs-lookup"><span data-stu-id="ac06f-312">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ac06f-313">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-313">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ac06f-314">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-314">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ac06f-315">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-315">15 seconds</span></span> | <span data-ttu-id="ac06f-316">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="ac06f-316">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ac06f-317">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="ac06f-317">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ac06f-318">如果用戶端未在伺服器上設定的 `ClientTimeoutInterval` 中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="ac06f-318">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="ac06f-319">在 .NET 用戶端中，timeout 值會指定為 `TimeSpan` 值。</span><span class="sxs-lookup"><span data-stu-id="ac06f-319">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ac06f-320">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ac06f-320">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ac06f-321">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-321">Option</span></span> | <span data-ttu-id="ac06f-322">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-322">Default value</span></span> | <span data-ttu-id="ac06f-323">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-323">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ac06f-324">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="ac06f-324">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ac06f-325">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="ac06f-325">Timeout for server activity.</span></span> <span data-ttu-id="ac06f-326">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="ac06f-326">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ac06f-327">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac06f-327">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ac06f-328">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="ac06f-328">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="ac06f-329">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="ac06f-329">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="ac06f-330">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="ac06f-330">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ac06f-331">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="ac06f-331">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ac06f-332">如果用戶端未在伺服器上設定的 `ClientTimeoutInterval` 中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="ac06f-332">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ac06f-333">Java</span><span class="sxs-lookup"><span data-stu-id="ac06f-333">Java</span></span>](#tab/java)

| <span data-ttu-id="ac06f-334">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-334">Option</span></span> | <span data-ttu-id="ac06f-335">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-335">Default value</span></span> | <span data-ttu-id="ac06f-336">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-336">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ac06f-337">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ac06f-337">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ac06f-338">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="ac06f-338">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ac06f-339">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="ac06f-339">Timeout for server activity.</span></span> <span data-ttu-id="ac06f-340">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="ac06f-340">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ac06f-341">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac06f-341">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ac06f-342">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="ac06f-342">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ac06f-343">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-343">15 seconds</span></span> | <span data-ttu-id="ac06f-344">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="ac06f-344">Timeout for initial server handshake.</span></span> <span data-ttu-id="ac06f-345">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="ac06f-345">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ac06f-346">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-346">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ac06f-347">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-347">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="ac06f-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="ac06f-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="ac06f-349">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="ac06f-349">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="ac06f-350">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="ac06f-350">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ac06f-351">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="ac06f-351">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ac06f-352">如果用戶端未在伺服器上設定的 `ClientTimeoutInterval` 中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="ac06f-352">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="ac06f-353">.NET</span><span class="sxs-lookup"><span data-stu-id="ac06f-353">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ac06f-354">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-354">Option</span></span> | <span data-ttu-id="ac06f-355">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-355">Default value</span></span> | <span data-ttu-id="ac06f-356">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-356">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ac06f-357">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="ac06f-357">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ac06f-358">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="ac06f-358">Timeout for server activity.</span></span> <span data-ttu-id="ac06f-359">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（在 JavaScript 中`onclose`）。</span><span class="sxs-lookup"><span data-stu-id="ac06f-359">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ac06f-360">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac06f-360">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ac06f-361">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="ac06f-361">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ac06f-362">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-362">15 seconds</span></span> | <span data-ttu-id="ac06f-363">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="ac06f-363">Timeout for initial server handshake.</span></span> <span data-ttu-id="ac06f-364">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（在 JavaScript 中`onclose`）。</span><span class="sxs-lookup"><span data-stu-id="ac06f-364">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ac06f-365">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-365">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ac06f-366">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-366">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="ac06f-367">在 .NET 用戶端中，timeout 值會指定為 `TimeSpan` 值。</span><span class="sxs-lookup"><span data-stu-id="ac06f-367">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ac06f-368">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ac06f-368">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ac06f-369">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-369">Option</span></span> | <span data-ttu-id="ac06f-370">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-370">Default value</span></span> | <span data-ttu-id="ac06f-371">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-371">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ac06f-372">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="ac06f-372">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ac06f-373">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="ac06f-373">Timeout for server activity.</span></span> <span data-ttu-id="ac06f-374">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="ac06f-374">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ac06f-375">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac06f-375">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ac06f-376">建議的值是至少為伺服器 `KeepAliveInterval` 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="ac06f-376">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ac06f-377">Java</span><span class="sxs-lookup"><span data-stu-id="ac06f-377">Java</span></span>](#tab/java)

| <span data-ttu-id="ac06f-378">選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-378">Option</span></span> | <span data-ttu-id="ac06f-379">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-379">Default value</span></span> | <span data-ttu-id="ac06f-380">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-380">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ac06f-381">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ac06f-381">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ac06f-382">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="ac06f-382">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ac06f-383">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="ac06f-383">Timeout for server activity.</span></span> <span data-ttu-id="ac06f-384">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="ac06f-384">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ac06f-385">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac06f-385">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ac06f-386">建議的值是伺服器的 `KeepAliveInterval` 值至少為兩倍的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="ac06f-386">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ac06f-387">15 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-387">15 seconds</span></span> | <span data-ttu-id="ac06f-388">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="ac06f-388">Timeout for initial server handshake.</span></span> <span data-ttu-id="ac06f-389">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="ac06f-389">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ac06f-390">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-390">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ac06f-391">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="ac06f-391">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="ac06f-392">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-392">Configure additional options</span></span>

<span data-ttu-id="ac06f-393">您可以在 `HubConnectionBuilder` 上或在 JAVA 用戶端的 `HttpHubConnectionBuilder` 上，于 `WithUrl` （在 JavaScript 中`withUrl`）方法中設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="ac06f-393">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="ac06f-394">.NET</span><span class="sxs-lookup"><span data-stu-id="ac06f-394">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ac06f-395">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-395">.NET Option</span></span> |  <span data-ttu-id="ac06f-396">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-396">Default value</span></span> | <span data-ttu-id="ac06f-397">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-397">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="ac06f-398">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="ac06f-398">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="ac06f-399">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="ac06f-399">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ac06f-400">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="ac06f-400">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ac06f-401">使用 Azure SignalR 服務時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-401">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="ac06f-402">Empty</span><span class="sxs-lookup"><span data-stu-id="ac06f-402">Empty</span></span> | <span data-ttu-id="ac06f-403">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="ac06f-403">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="ac06f-404">Empty</span><span class="sxs-lookup"><span data-stu-id="ac06f-404">Empty</span></span> | <span data-ttu-id="ac06f-405">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="ac06f-405">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="ac06f-406">Empty</span><span class="sxs-lookup"><span data-stu-id="ac06f-406">Empty</span></span> | <span data-ttu-id="ac06f-407">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="ac06f-407">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="ac06f-408">5 秒</span><span class="sxs-lookup"><span data-stu-id="ac06f-408">5 seconds</span></span> | <span data-ttu-id="ac06f-409">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="ac06f-409">WebSockets only.</span></span> <span data-ttu-id="ac06f-410">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="ac06f-410">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="ac06f-411">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="ac06f-411">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="ac06f-412">Empty</span><span class="sxs-lookup"><span data-stu-id="ac06f-412">Empty</span></span> | <span data-ttu-id="ac06f-413">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="ac06f-413">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="ac06f-414">委派，可以用來設定或取代用來傳送 HTTP 要求的 `HttpMessageHandler`。</span><span class="sxs-lookup"><span data-stu-id="ac06f-414">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="ac06f-415">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="ac06f-415">Not used for WebSocket connections.</span></span> <span data-ttu-id="ac06f-416">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="ac06f-416">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="ac06f-417">請修改該預設值的設定並將其傳回，或傳回新的 `HttpMessageHandler` 實例。</span><span class="sxs-lookup"><span data-stu-id="ac06f-417">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="ac06f-418">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="ac06f-418">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="ac06f-419">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="ac06f-419">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="ac06f-420">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="ac06f-420">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="ac06f-421">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="ac06f-421">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="ac06f-422">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="ac06f-422">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="ac06f-423">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="ac06f-423">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ac06f-424">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ac06f-424">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ac06f-425">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-425">JavaScript Option</span></span> | <span data-ttu-id="ac06f-426">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-426">Default Value</span></span> | <span data-ttu-id="ac06f-427">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-427">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="ac06f-428">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="ac06f-428">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="ac06f-429">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="ac06f-429">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ac06f-430">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="ac06f-430">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ac06f-431">使用 Azure SignalR 服務時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-431">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ac06f-432">Java</span><span class="sxs-lookup"><span data-stu-id="ac06f-432">Java</span></span>](#tab/java)

| <span data-ttu-id="ac06f-433">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="ac06f-433">Java Option</span></span> | <span data-ttu-id="ac06f-434">預設值</span><span class="sxs-lookup"><span data-stu-id="ac06f-434">Default Value</span></span> | <span data-ttu-id="ac06f-435">描述</span><span class="sxs-lookup"><span data-stu-id="ac06f-435">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="ac06f-436">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="ac06f-436">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="ac06f-437">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="ac06f-437">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ac06f-438">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="ac06f-438">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ac06f-439">使用 Azure SignalR 服務時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="ac06f-439">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="ac06f-440">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="ac06f-440">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="ac06f-441">Empty</span><span class="sxs-lookup"><span data-stu-id="ac06f-441">Empty</span></span> | <span data-ttu-id="ac06f-442">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="ac06f-442">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="ac06f-443">在 .NET 用戶端中，這些選項可以由提供給 `WithUrl`的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="ac06f-443">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="ac06f-444">在 JavaScript 用戶端中，可以在提供給 `withUrl`的 JavaScript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="ac06f-444">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="ac06f-445">在 JAVA 用戶端中，可以使用從 `HubConnectionBuilder.create("HUB URL")` 傳回的 `HttpHubConnectionBuilder` 上的方法來設定這些選項。</span><span class="sxs-lookup"><span data-stu-id="ac06f-445">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="ac06f-446">其他資源</span><span class="sxs-lookup"><span data-stu-id="ac06f-446">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
