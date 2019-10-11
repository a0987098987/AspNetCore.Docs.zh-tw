---
title: ASP.NET Core SignalR 設定
author: bradygaster
description: 瞭解如何設定 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 66f274fcda27392091de6b4be8c7221bc87b7585
ms.sourcegitcommit: c452e6af92e130413106c4863193f377cde4cd9c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246485"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="513e0-103">ASP.NET Core SignalR 設定</span><span class="sxs-lookup"><span data-stu-id="513e0-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="513e0-104">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="513e0-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="513e0-105">ASP.NET Core SignalR 支援兩種通訊協定來編碼訊息：[JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="513e0-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="513e0-106">每個通訊協定都有序列化設定選項。</span><span class="sxs-lookup"><span data-stu-id="513e0-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="513e0-107">您可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，在伺服器上設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="513e0-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="513e0-108">`AddJsonProtocol` 可以在[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)中的 `Startup.ConfigureServices` 之後加入。</span><span class="sxs-lookup"><span data-stu-id="513e0-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="513e0-109">@No__t-0 方法會接受一個可接收 `options` 物件的委派。</span><span class="sxs-lookup"><span data-stu-id="513e0-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="513e0-110">該物件上的[PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是 @no__t 1 @no__t 2 物件，可用來設定引數和傳回值的序列化。</span><span class="sxs-lookup"><span data-stu-id="513e0-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="513e0-111">如需詳細資訊，請參閱[system.web 檔](/dotnet/api/system.text.json)。</span><span class="sxs-lookup"><span data-stu-id="513e0-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="513e0-112">例如，若要設定序列化程式不變更屬性名稱的大小寫，而不是預設的 "camelCase" 名稱，請在 `Startup.ConfigureServices` 中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="513e0-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="513e0-113">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="513e0-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="513e0-114">必須匯入 `Microsoft.Extensions.DependencyInjection` 命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="513e0-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="513e0-115">切換至 Newtonsoft. Json</span><span class="sxs-lookup"><span data-stu-id="513e0-115">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="513e0-116">如果您需要 `Newtonsoft.Json` 的功能，但不支援 `System.Text.Json`，請參閱[切換至 Newtonsoft](xref:migration/22-to-30#switch-to-newtonsoftjson)。</span><span class="sxs-lookup"><span data-stu-id="513e0-116">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="513e0-117">您可以在伺服器上使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法來設定 JSON 序列化，這可在您的 `Startup.ConfigureServices` 方法中的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後加入。</span><span class="sxs-lookup"><span data-stu-id="513e0-117">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="513e0-118">@No__t-0 方法會接受一個可接收 `options` 物件的委派。</span><span class="sxs-lookup"><span data-stu-id="513e0-118">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="513e0-119">該物件上的[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性是一個可用於設定引數和傳回值序列化的 JSON.NET @no__t 1 物件。</span><span class="sxs-lookup"><span data-stu-id="513e0-119">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="513e0-120">如需詳細資訊，請參閱[JSON.NET 檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。</span><span class="sxs-lookup"><span data-stu-id="513e0-120">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="513e0-121">例如，若要設定序列化程式使用 "PascalCase" 屬性名稱，而不是預設的 "camelCase" 名稱，請在 `Startup.ConfigureServices` 中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="513e0-121">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="513e0-122">在 .NET 用戶端中，相同的 `AddJsonProtocol` 擴充方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上。</span><span class="sxs-lookup"><span data-stu-id="513e0-122">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="513e0-123">必須匯入 `Microsoft.Extensions.DependencyInjection` 命名空間，才能解析擴充方法：</span><span class="sxs-lookup"><span data-stu-id="513e0-123">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="513e0-124">此時不可能在 JavaScript 用戶端中設定 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="513e0-124">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="513e0-125">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="513e0-125">MessagePack serialization options</span></span>

<span data-ttu-id="513e0-126">MessagePack 序列化可以藉由提供委派給[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫來設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-126">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="513e0-127">如需詳細資訊，請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol) 。</span><span class="sxs-lookup"><span data-stu-id="513e0-127">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="513e0-128">此時不可能在 JavaScript 用戶端中設定 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="513e0-128">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="513e0-129">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="513e0-129">Configure server options</span></span>

<span data-ttu-id="513e0-130">下表說明設定 SignalR 中樞的選項：</span><span class="sxs-lookup"><span data-stu-id="513e0-130">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="513e0-131">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-131">Option</span></span> | <span data-ttu-id="513e0-132">Default Value</span><span class="sxs-lookup"><span data-stu-id="513e0-132">Default Value</span></span> | <span data-ttu-id="513e0-133">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-133">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="513e0-134">30 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-134">30 seconds</span></span> | <span data-ttu-id="513e0-135">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="513e0-135">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="513e0-136">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="513e0-136">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="513e0-137">建議的值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="513e0-137">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="513e0-138">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-138">15 seconds</span></span> | <span data-ttu-id="513e0-139">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="513e0-139">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="513e0-140">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-140">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="513e0-141">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="513e0-141">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="513e0-142">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-142">15 seconds</span></span> | <span data-ttu-id="513e0-143">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="513e0-143">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="513e0-144">變更 `KeepAliveInterval` 時，請變更用戶端上的 `ServerTimeout` @ no__t-2 @ no__t-3 設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-144">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="513e0-145">建議的 `ServerTimeout` @ no__t-1 @ no__t-2 值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="513e0-145">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="513e0-146">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="513e0-146">All installed protocols</span></span> | <span data-ttu-id="513e0-147">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="513e0-147">Protocols supported by this hub.</span></span> <span data-ttu-id="513e0-148">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="513e0-148">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="513e0-149">如果 `true`，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="513e0-149">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="513e0-150">預設值為 `false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="513e0-150">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="513e0-151">可以針對用戶端上傳資料流程進行緩衝處理的專案數上限。</span><span class="sxs-lookup"><span data-stu-id="513e0-151">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="513e0-152">若達到此限制，則在伺服器處理資料流程專案之前，會封鎖調用的處理。</span><span class="sxs-lookup"><span data-stu-id="513e0-152">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="513e0-153">32 KB</span><span class="sxs-lookup"><span data-stu-id="513e0-153">32 KB</span></span> | <span data-ttu-id="513e0-154">單一傳入中樞訊息的大小上限。</span><span class="sxs-lookup"><span data-stu-id="513e0-154">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="513e0-155">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-155">Option</span></span> | <span data-ttu-id="513e0-156">Default Value</span><span class="sxs-lookup"><span data-stu-id="513e0-156">Default Value</span></span> | <span data-ttu-id="513e0-157">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-157">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="513e0-158">30 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-158">30 seconds</span></span> | <span data-ttu-id="513e0-159">如果用戶端未在此間隔內收到訊息（包括 keep-alive），伺服器會將它視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="513e0-159">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="513e0-160">這可能需要比此逾時間隔更長的時間，讓用戶端實際被標示為已中斷連線，因為這是如何實行的。</span><span class="sxs-lookup"><span data-stu-id="513e0-160">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="513e0-161">建議的值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="513e0-161">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="513e0-162">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-162">15 seconds</span></span> | <span data-ttu-id="513e0-163">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="513e0-163">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="513e0-164">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-164">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="513e0-165">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="513e0-165">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="513e0-166">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-166">15 seconds</span></span> | <span data-ttu-id="513e0-167">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="513e0-167">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="513e0-168">變更 `KeepAliveInterval` 時，請變更用戶端上的 `ServerTimeout` @ no__t-2 @ no__t-3 設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-168">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="513e0-169">建議的 `ServerTimeout` @ no__t-1 @ no__t-2 值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="513e0-169">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="513e0-170">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="513e0-170">All installed protocols</span></span> | <span data-ttu-id="513e0-171">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="513e0-171">Protocols supported by this hub.</span></span> <span data-ttu-id="513e0-172">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="513e0-172">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="513e0-173">如果 `true`，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="513e0-173">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="513e0-174">預設值為 `false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="513e0-174">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="513e0-175">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-175">Option</span></span> | <span data-ttu-id="513e0-176">Default Value</span><span class="sxs-lookup"><span data-stu-id="513e0-176">Default Value</span></span> | <span data-ttu-id="513e0-177">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-177">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="513e0-178">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-178">15 seconds</span></span> | <span data-ttu-id="513e0-179">如果用戶端未在此時間間隔內傳送初始交握訊息，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="513e0-179">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="513e0-180">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-180">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="513e0-181">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="513e0-181">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="513e0-182">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-182">15 seconds</span></span> | <span data-ttu-id="513e0-183">如果伺服器未在此間隔內傳送訊息，則會自動傳送 ping 訊息，讓連接保持開啟。</span><span class="sxs-lookup"><span data-stu-id="513e0-183">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="513e0-184">變更 `KeepAliveInterval` 時，請變更用戶端上的 `ServerTimeout` @ no__t-2 @ no__t-3 設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-184">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="513e0-185">建議的 `ServerTimeout` @ no__t-1 @ no__t-2 值為 `KeepAliveInterval` 值的雙精度浮點數。</span><span class="sxs-lookup"><span data-stu-id="513e0-185">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="513e0-186">所有已安裝的通訊協定</span><span class="sxs-lookup"><span data-stu-id="513e0-186">All installed protocols</span></span> | <span data-ttu-id="513e0-187">此中樞支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="513e0-187">Protocols supported by this hub.</span></span> <span data-ttu-id="513e0-188">根據預設，允許在伺服器上註冊的所有通訊協定，但是可以從這份清單中移除通訊協定，以停用個別中樞的特定通訊協定。</span><span class="sxs-lookup"><span data-stu-id="513e0-188">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="513e0-189">如果 `true`，當中樞方法擲回例外狀況時，會將詳細的例外狀況訊息傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="513e0-189">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="513e0-190">預設值為 `false`，因為這些例外狀況訊息可能包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="513e0-190">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="513e0-191">您可以為所有中樞設定選項，方法是在 `Startup.ConfigureServices` 中提供選項委派給 `AddSignalR` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="513e0-191">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="513e0-192">單一中樞的選項會覆寫 `AddSignalR` 中提供的全域選項，而且可以使用 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*> 來設定：</span><span class="sxs-lookup"><span data-stu-id="513e0-192">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="513e0-193">Advanced HTTP 設定選項</span><span class="sxs-lookup"><span data-stu-id="513e0-193">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="513e0-194">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-194">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="513e0-195">這些選項的設定方式是將委派傳遞至 `Startup.Configure` 中的[MapHub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) 。</span><span class="sxs-lookup"><span data-stu-id="513e0-195">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="513e0-196">使用 `HttpConnectionDispatcherOptions` 來設定與傳輸和記憶體緩衝區管理相關的 advanced 設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-196">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="513e0-197">這些選項的設定方式是將委派傳遞至 `Startup.Configure` 中的[MapHub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) 。</span><span class="sxs-lookup"><span data-stu-id="513e0-197">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="513e0-198">下表說明設定 ASP.NET Core SignalR 的 advanced HTTP 選項的選項：</span><span class="sxs-lookup"><span data-stu-id="513e0-198">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="513e0-199">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-199">Option</span></span> | <span data-ttu-id="513e0-200">Default Value</span><span class="sxs-lookup"><span data-stu-id="513e0-200">Default Value</span></span> | <span data-ttu-id="513e0-201">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-201">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="513e0-202">32 KB</span><span class="sxs-lookup"><span data-stu-id="513e0-202">32 KB</span></span> | <span data-ttu-id="513e0-203">在套用背壓之前，伺服器會緩衝的用戶端接收的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="513e0-203">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="513e0-204">增加此值可讓伺服器更快速地接收較大的訊息，而不需套用背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="513e0-204">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="513e0-205">自動從套用至中樞類別的 `Authorize` 屬性收集資料。</span><span class="sxs-lookup"><span data-stu-id="513e0-205">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="513e0-206">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="513e0-206">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="513e0-207">32 KB</span><span class="sxs-lookup"><span data-stu-id="513e0-207">32 KB</span></span> | <span data-ttu-id="513e0-208">在觀察到背壓之前，伺服器會緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="513e0-208">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="513e0-209">增加此值可讓伺服器更快速地緩衝較大的訊息，而不需等待背壓，但可能會增加記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="513e0-209">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="513e0-210">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="513e0-210">All Transports are enabled.</span></span> | <span data-ttu-id="513e0-211">@No__t-0 值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="513e0-211">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="513e0-212">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="513e0-212">See below.</span></span> | <span data-ttu-id="513e0-213">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="513e0-213">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="513e0-214">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="513e0-214">See below.</span></span> | <span data-ttu-id="513e0-215">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="513e0-215">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="513e0-216">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-216">Option</span></span> | <span data-ttu-id="513e0-217">Default Value</span><span class="sxs-lookup"><span data-stu-id="513e0-217">Default Value</span></span> | <span data-ttu-id="513e0-218">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-218">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="513e0-219">32 KB</span><span class="sxs-lookup"><span data-stu-id="513e0-219">32 KB</span></span> | <span data-ttu-id="513e0-220">從用戶端接收伺服器緩衝區的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="513e0-220">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="513e0-221">增加這個值可讓伺服器接收較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="513e0-221">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="513e0-222">自動從套用至中樞類別的 `Authorize` 屬性收集資料。</span><span class="sxs-lookup"><span data-stu-id="513e0-222">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="513e0-223">用來判斷是否授權用戶端連線到中樞的[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件清單。</span><span class="sxs-lookup"><span data-stu-id="513e0-223">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="513e0-224">32 KB</span><span class="sxs-lookup"><span data-stu-id="513e0-224">32 KB</span></span> | <span data-ttu-id="513e0-225">由伺服器所緩衝的應用程式所傳送的最大位元組數目。</span><span class="sxs-lookup"><span data-stu-id="513e0-225">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="513e0-226">增加這個值可讓伺服器傳送較大的訊息，但可能會對記憶體耗用量造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="513e0-226">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="513e0-227">所有傳輸都已啟用。</span><span class="sxs-lookup"><span data-stu-id="513e0-227">All Transports are enabled.</span></span> | <span data-ttu-id="513e0-228">@No__t-0 值的位旗標列舉，可以限制用戶端可用來連接的傳輸。</span><span class="sxs-lookup"><span data-stu-id="513e0-228">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="513e0-229">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="513e0-229">See below.</span></span> | <span data-ttu-id="513e0-230">長輪詢傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="513e0-230">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="513e0-231">請參閱下方。</span><span class="sxs-lookup"><span data-stu-id="513e0-231">See below.</span></span> | <span data-ttu-id="513e0-232">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="513e0-232">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="513e0-233">長輪詢傳輸具有其他選項，可以使用 `LongPolling` 屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="513e0-233">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="513e0-234">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-234">Option</span></span> | <span data-ttu-id="513e0-235">Default Value</span><span class="sxs-lookup"><span data-stu-id="513e0-235">Default Value</span></span> | <span data-ttu-id="513e0-236">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-236">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="513e0-237">90秒</span><span class="sxs-lookup"><span data-stu-id="513e0-237">90 seconds</span></span> | <span data-ttu-id="513e0-238">在終止單一輪詢要求之前，伺服器等候訊息傳送至用戶端的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="513e0-238">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="513e0-239">降低此值會導致用戶端更頻繁地發出新的輪詢要求。</span><span class="sxs-lookup"><span data-stu-id="513e0-239">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="513e0-240">WebSocket 傳輸具有其他選項，可以使用 `WebSockets` 屬性來設定：</span><span class="sxs-lookup"><span data-stu-id="513e0-240">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="513e0-241">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-241">Option</span></span> | <span data-ttu-id="513e0-242">Default Value</span><span class="sxs-lookup"><span data-stu-id="513e0-242">Default Value</span></span> | <span data-ttu-id="513e0-243">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-243">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="513e0-244">5 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-244">5 seconds</span></span> | <span data-ttu-id="513e0-245">伺服器關閉之後，如果用戶端無法在此時間間隔內關閉，連接就會終止。</span><span class="sxs-lookup"><span data-stu-id="513e0-245">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="513e0-246">可以用來將 `Sec-WebSocket-Protocol` 標頭設定為自訂值的委派。</span><span class="sxs-lookup"><span data-stu-id="513e0-246">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="513e0-247">委派會接收用戶端要求的值做為輸入，而且預期會傳回所需的值。</span><span class="sxs-lookup"><span data-stu-id="513e0-247">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="513e0-248">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="513e0-248">Configure client options</span></span>

<span data-ttu-id="513e0-249">用戶端選項可以在 `HubConnectionBuilder` 類型上設定（在 .NET 和 JavaScript 用戶端中提供）。</span><span class="sxs-lookup"><span data-stu-id="513e0-249">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="513e0-250">它也可在 JAVA 用戶端中使用，但 @no__t 0 子類別則包含 builder 設定選項，以及 `HubConnection` 本身。</span><span class="sxs-lookup"><span data-stu-id="513e0-250">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="513e0-251">設定記錄</span><span class="sxs-lookup"><span data-stu-id="513e0-251">Configure logging</span></span>

<span data-ttu-id="513e0-252">記錄是在 .NET 用戶端中使用 `ConfigureLogging` 方法來設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-252">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="513e0-253">記錄提供者和篩選器可以用與伺服器上相同的方式進行註冊。</span><span class="sxs-lookup"><span data-stu-id="513e0-253">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="513e0-254">如需詳細資訊，請參閱[ASP.NET Core 檔中的登入](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="513e0-254">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="513e0-255">若要註冊記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="513e0-255">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="513e0-256">如需完整清單，請參閱檔的[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)一節。</span><span class="sxs-lookup"><span data-stu-id="513e0-256">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="513e0-257">例如，若要啟用主控台記錄，請安裝 `Microsoft.Extensions.Logging.Console` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="513e0-257">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="513e0-258">呼叫 @no__t 0 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="513e0-258">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="513e0-259">在 JavaScript 用戶端中，有類似的 `configureLogging` 方法存在。</span><span class="sxs-lookup"><span data-stu-id="513e0-259">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="513e0-260">提供 `LogLevel` 值，表示要產生的記錄訊息的最小層級。</span><span class="sxs-lookup"><span data-stu-id="513e0-260">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="513e0-261">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="513e0-261">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="513e0-262">您也可以提供代表記錄層級名稱的 `string` 值，而不是 @no__t 0 值。</span><span class="sxs-lookup"><span data-stu-id="513e0-262">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="513e0-263">在您無法存取 @no__t 0 常數的環境中設定 SignalR 記錄時，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="513e0-263">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="513e0-264">下表列出可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="513e0-264">The following table lists the available log levels.</span></span> <span data-ttu-id="513e0-265">您提供給 `configureLogging` 的值會設定將記錄的**最低**記錄層級。</span><span class="sxs-lookup"><span data-stu-id="513e0-265">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="513e0-266">記錄在此層級的訊息，**或在資料表中所列的層級**將會記錄下來。</span><span class="sxs-lookup"><span data-stu-id="513e0-266">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="513e0-267">String</span><span class="sxs-lookup"><span data-stu-id="513e0-267">String</span></span>                      | <span data-ttu-id="513e0-268">LogLevel</span><span class="sxs-lookup"><span data-stu-id="513e0-268">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="513e0-269">`info`**或**`information`</span><span class="sxs-lookup"><span data-stu-id="513e0-269">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="513e0-270">`warn`**或**`warning`</span><span class="sxs-lookup"><span data-stu-id="513e0-270">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="513e0-271">若要完全停用記錄，請在 `configureLogging` 方法中指定 `signalR.LogLevel.None`。</span><span class="sxs-lookup"><span data-stu-id="513e0-271">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="513e0-272">如需記錄的詳細資訊，請參閱[SignalR 診斷檔](xref:signalr/diagnostics)。</span><span class="sxs-lookup"><span data-stu-id="513e0-272">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="513e0-273">SignalR JAVA 用戶端會使用[SLF4J](https://www.slf4j.org/)程式庫進行記錄。</span><span class="sxs-lookup"><span data-stu-id="513e0-273">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="513e0-274">它是高階記錄 API，可讓程式庫的使用者藉由帶入特定的記錄相依性來選擇自己的特定記錄執行。</span><span class="sxs-lookup"><span data-stu-id="513e0-274">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="513e0-275">下列程式碼片段顯示如何搭配 SignalR JAVA 用戶端使用 `java.util.logging`。</span><span class="sxs-lookup"><span data-stu-id="513e0-275">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="513e0-276">如果您未在相依性中設定記錄，SLF4J 會載入預設的無作業記錄器，並顯示下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="513e0-276">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="513e0-277">可以放心地忽略這種情況。</span><span class="sxs-lookup"><span data-stu-id="513e0-277">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="513e0-278">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="513e0-278">Configure allowed transports</span></span>

<span data-ttu-id="513e0-279">SignalR 所使用的傳輸可以在 `WithUrl` 呼叫中設定（以 JavaScript `withUrl`）。</span><span class="sxs-lookup"><span data-stu-id="513e0-279">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="513e0-280">@No__t-0 值的位 OR 可以用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="513e0-280">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="513e0-281">預設會啟用所有傳輸。</span><span class="sxs-lookup"><span data-stu-id="513e0-281">All transports are enabled by default.</span></span>

<span data-ttu-id="513e0-282">例如，若要停用伺服器傳送的事件傳輸，但允許 Websocket 和較長的輪詢連接：</span><span class="sxs-lookup"><span data-stu-id="513e0-282">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="513e0-283">在 JavaScript 用戶端中，傳輸是藉由在提供給 `withUrl` 的 options 物件上設定 `transport` 欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="513e0-283">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="513e0-284">在此版本的 JAVA 用戶端 websocket 是唯一可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="513e0-284">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="513e0-285">在 JAVA 用戶端中，會使用 `HttpHubConnectionBuilder` 上的 `withTransport` 方法來選取傳輸。</span><span class="sxs-lookup"><span data-stu-id="513e0-285">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="513e0-286">JAVA 用戶端預設為使用 Websocket 傳輸。</span><span class="sxs-lookup"><span data-stu-id="513e0-286">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="513e0-287">SignalR JAVA 用戶端尚不支援傳輸回退。</span><span class="sxs-lookup"><span data-stu-id="513e0-287">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="513e0-288">設定持有人驗證</span><span class="sxs-lookup"><span data-stu-id="513e0-288">Configure bearer authentication</span></span>

<span data-ttu-id="513e0-289">若要提供驗證資料以及 SignalR 要求，請使用 `AccessTokenProvider` 選項（JavaScript 中的 `accessTokenFactory`）來指定函式，以傳回所需的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="513e0-289">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="513e0-290">在 .NET 用戶端中，此存取權杖會當做 HTTP 「持有人驗證」權杖（使用 `Bearer` 類型的 `Authorization` 標頭）傳入。</span><span class="sxs-lookup"><span data-stu-id="513e0-290">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="513e0-291">在 JavaScript 用戶端中，存取權杖會用來做為持有人權杖，**但**在少數情況下，瀏覽器 api 會限制套用標頭的功能（特別是在伺服器傳送的事件和 websocket 要求中）。</span><span class="sxs-lookup"><span data-stu-id="513e0-291">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="513e0-292">在這些情況下，存取權杖會以查詢字串值的形式提供 `access_token`。</span><span class="sxs-lookup"><span data-stu-id="513e0-292">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="513e0-293">在 .NET 用戶端中，可以使用 `WithUrl` 中的選項委派來指定 `AccessTokenProvider` 選項：</span><span class="sxs-lookup"><span data-stu-id="513e0-293">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="513e0-294">在 JavaScript 用戶端中，存取權杖是藉由在 `withUrl` 的 options 物件上設定 `accessTokenFactory` 欄位來設定：</span><span class="sxs-lookup"><span data-stu-id="513e0-294">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="513e0-295">在 SignalR JAVA 用戶端中，您可以藉由提供存取權杖 factory 給[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)，設定要用於驗證的持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="513e0-295">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="513e0-296">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava) [單一 @ no__t-3String >](https://reactivex.io/documentation/single.html)。</span><span class="sxs-lookup"><span data-stu-id="513e0-296">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="513e0-297">只要呼叫[單一. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您就可以撰寫邏輯來產生用戶端的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="513e0-297">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="513e0-298">設定 timeout 和 keep-alive 選項</span><span class="sxs-lookup"><span data-stu-id="513e0-298">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="513e0-299">設定 timeout 和 keep-alive 行為的其他選項可用於 `HubConnection` 物件本身：</span><span class="sxs-lookup"><span data-stu-id="513e0-299">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="513e0-300">.NET</span><span class="sxs-lookup"><span data-stu-id="513e0-300">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="513e0-301">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-301">Option</span></span> | <span data-ttu-id="513e0-302">預設值</span><span class="sxs-lookup"><span data-stu-id="513e0-302">Default value</span></span> | <span data-ttu-id="513e0-303">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-303">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="513e0-304">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="513e0-304">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="513e0-305">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="513e0-305">Timeout for server activity.</span></span> <span data-ttu-id="513e0-306">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（在 JavaScript 中為 `onclose`）。</span><span class="sxs-lookup"><span data-stu-id="513e0-306">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="513e0-307">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="513e0-307">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="513e0-308">建議的值是至少兩個伺服器的 @no__t 0 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="513e0-308">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="513e0-309">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-309">15 seconds</span></span> | <span data-ttu-id="513e0-310">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="513e0-310">Timeout for initial server handshake.</span></span> <span data-ttu-id="513e0-311">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（在 JavaScript 中為 `onclose`）。</span><span class="sxs-lookup"><span data-stu-id="513e0-311">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="513e0-312">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-312">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="513e0-313">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="513e0-313">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="513e0-314">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-314">15 seconds</span></span> | <span data-ttu-id="513e0-315">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="513e0-315">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="513e0-316">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="513e0-316">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="513e0-317">如果用戶端未在伺服器上的 @no__t 0 設定中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="513e0-317">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="513e0-318">在 .NET 用戶端中，timeout 值會指定為 @no__t 0 值。</span><span class="sxs-lookup"><span data-stu-id="513e0-318">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="513e0-319">JavaScript</span><span class="sxs-lookup"><span data-stu-id="513e0-319">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="513e0-320">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-320">Option</span></span> | <span data-ttu-id="513e0-321">預設值</span><span class="sxs-lookup"><span data-stu-id="513e0-321">Default value</span></span> | <span data-ttu-id="513e0-322">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-322">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="513e0-323">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="513e0-323">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="513e0-324">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="513e0-324">Timeout for server activity.</span></span> <span data-ttu-id="513e0-325">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="513e0-325">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="513e0-326">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="513e0-326">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="513e0-327">建議的值是至少兩個伺服器的 @no__t 0 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="513e0-327">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="513e0-328">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="513e0-328">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="513e0-329">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="513e0-329">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="513e0-330">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="513e0-330">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="513e0-331">如果用戶端未在伺服器上的 @no__t 0 設定中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="513e0-331">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="513e0-332">Java</span><span class="sxs-lookup"><span data-stu-id="513e0-332">Java</span></span>](#tab/java)

| <span data-ttu-id="513e0-333">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-333">Option</span></span> | <span data-ttu-id="513e0-334">預設值</span><span class="sxs-lookup"><span data-stu-id="513e0-334">Default value</span></span> | <span data-ttu-id="513e0-335">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-335">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="513e0-336">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="513e0-336">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="513e0-337">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="513e0-337">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="513e0-338">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="513e0-338">Timeout for server activity.</span></span> <span data-ttu-id="513e0-339">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="513e0-339">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="513e0-340">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="513e0-340">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="513e0-341">建議的值是至少兩個伺服器的 @no__t 0 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="513e0-341">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="513e0-342">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-342">15 seconds</span></span> | <span data-ttu-id="513e0-343">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="513e0-343">Timeout for initial server handshake.</span></span> <span data-ttu-id="513e0-344">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="513e0-344">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="513e0-345">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-345">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="513e0-346">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="513e0-346">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="513e0-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="513e0-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="513e0-348">15秒（15000毫秒）</span><span class="sxs-lookup"><span data-stu-id="513e0-348">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="513e0-349">決定用戶端傳送 ping 訊息的間隔。</span><span class="sxs-lookup"><span data-stu-id="513e0-349">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="513e0-350">從用戶端傳送任何訊息時，會將計時器重設為間隔的開始。</span><span class="sxs-lookup"><span data-stu-id="513e0-350">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="513e0-351">如果用戶端未在伺服器上的 @no__t 0 設定中傳送訊息，伺服器會將用戶端視為已中斷連線。</span><span class="sxs-lookup"><span data-stu-id="513e0-351">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="513e0-352">.NET</span><span class="sxs-lookup"><span data-stu-id="513e0-352">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="513e0-353">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-353">Option</span></span> | <span data-ttu-id="513e0-354">預設值</span><span class="sxs-lookup"><span data-stu-id="513e0-354">Default value</span></span> | <span data-ttu-id="513e0-355">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-355">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="513e0-356">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="513e0-356">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="513e0-357">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="513e0-357">Timeout for server activity.</span></span> <span data-ttu-id="513e0-358">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `Closed` 事件（在 JavaScript 中為 `onclose`）。</span><span class="sxs-lookup"><span data-stu-id="513e0-358">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="513e0-359">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="513e0-359">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="513e0-360">建議的值是至少兩個伺服器的 @no__t 0 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="513e0-360">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="513e0-361">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-361">15 seconds</span></span> | <span data-ttu-id="513e0-362">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="513e0-362">Timeout for initial server handshake.</span></span> <span data-ttu-id="513e0-363">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `Closed` 事件（在 JavaScript 中為 `onclose`）。</span><span class="sxs-lookup"><span data-stu-id="513e0-363">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="513e0-364">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-364">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="513e0-365">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="513e0-365">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="513e0-366">在 .NET 用戶端中，timeout 值會指定為 @no__t 0 值。</span><span class="sxs-lookup"><span data-stu-id="513e0-366">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="513e0-367">JavaScript</span><span class="sxs-lookup"><span data-stu-id="513e0-367">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="513e0-368">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-368">Option</span></span> | <span data-ttu-id="513e0-369">預設值</span><span class="sxs-lookup"><span data-stu-id="513e0-369">Default value</span></span> | <span data-ttu-id="513e0-370">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-370">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="513e0-371">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="513e0-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="513e0-372">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="513e0-372">Timeout for server activity.</span></span> <span data-ttu-id="513e0-373">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onclose` 事件。</span><span class="sxs-lookup"><span data-stu-id="513e0-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="513e0-374">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="513e0-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="513e0-375">建議的值是至少兩個伺服器的 @no__t 0 值的數位，以允許時間到達 ping。</span><span class="sxs-lookup"><span data-stu-id="513e0-375">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="513e0-376">Java</span><span class="sxs-lookup"><span data-stu-id="513e0-376">Java</span></span>](#tab/java)

| <span data-ttu-id="513e0-377">選項</span><span class="sxs-lookup"><span data-stu-id="513e0-377">Option</span></span> | <span data-ttu-id="513e0-378">預設值</span><span class="sxs-lookup"><span data-stu-id="513e0-378">Default value</span></span> | <span data-ttu-id="513e0-379">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-379">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="513e0-380">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="513e0-380">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="513e0-381">30秒（30000毫秒）</span><span class="sxs-lookup"><span data-stu-id="513e0-381">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="513e0-382">伺服器活動的超時時間。</span><span class="sxs-lookup"><span data-stu-id="513e0-382">Timeout for server activity.</span></span> <span data-ttu-id="513e0-383">如果伺服器未在此間隔內傳送訊息，用戶端會將伺服器視為已中斷連線，並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="513e0-383">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="513e0-384">這個值必須夠大，才能從伺服器傳送 ping 訊息 **，並**在逾時間隔內接收用戶端。</span><span class="sxs-lookup"><span data-stu-id="513e0-384">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="513e0-385">建議的值是至少為伺服器的 @no__t 0 值的數位，以允許時間讓 ping 抵達。</span><span class="sxs-lookup"><span data-stu-id="513e0-385">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="513e0-386">15 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-386">15 seconds</span></span> | <span data-ttu-id="513e0-387">初始伺服器交握的超時時間。</span><span class="sxs-lookup"><span data-stu-id="513e0-387">Timeout for initial server handshake.</span></span> <span data-ttu-id="513e0-388">如果伺服器未在此間隔內傳送交握回應，用戶端會取消交握並觸發 `onClose` 事件。</span><span class="sxs-lookup"><span data-stu-id="513e0-388">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="513e0-389">這是一種只有在因網路延遲嚴重而發生交握逾時錯誤時，才應該修改的「高級」設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-389">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="513e0-390">如需交握程式的詳細資訊，請參閱[SignalR 中樞通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。</span><span class="sxs-lookup"><span data-stu-id="513e0-390">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="513e0-391">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="513e0-391">Configure additional options</span></span>

<span data-ttu-id="513e0-392">您可以在 `HubConnectionBuilder` 上或在 JAVA 用戶端的 `HttpHubConnectionBuilder` 上的各種設定 Api 上，以 `WithUrl` （JavaScript 中的 `withUrl`）方法來設定其他選項：</span><span class="sxs-lookup"><span data-stu-id="513e0-392">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="513e0-393">.NET</span><span class="sxs-lookup"><span data-stu-id="513e0-393">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="513e0-394">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="513e0-394">.NET Option</span></span> |  <span data-ttu-id="513e0-395">預設值</span><span class="sxs-lookup"><span data-stu-id="513e0-395">Default value</span></span> | <span data-ttu-id="513e0-396">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-396">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="513e0-397">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="513e0-397">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="513e0-398">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="513e0-398">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="513e0-399">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="513e0-399">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="513e0-400">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-400">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="513e0-401">Empty</span><span class="sxs-lookup"><span data-stu-id="513e0-401">Empty</span></span> | <span data-ttu-id="513e0-402">要傳送以驗證要求的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="513e0-402">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="513e0-403">Empty</span><span class="sxs-lookup"><span data-stu-id="513e0-403">Empty</span></span> | <span data-ttu-id="513e0-404">要與每個 HTTP 要求一起傳送的 HTTP cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="513e0-404">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="513e0-405">Empty</span><span class="sxs-lookup"><span data-stu-id="513e0-405">Empty</span></span> | <span data-ttu-id="513e0-406">每個 HTTP 要求傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="513e0-406">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="513e0-407">5 秒</span><span class="sxs-lookup"><span data-stu-id="513e0-407">5 seconds</span></span> | <span data-ttu-id="513e0-408">僅限 Websocket。</span><span class="sxs-lookup"><span data-stu-id="513e0-408">WebSockets only.</span></span> <span data-ttu-id="513e0-409">關閉伺服器以認可關閉要求之後，用戶端等待的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="513e0-409">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="513e0-410">如果伺服器在這段時間內未認可關閉，用戶端就會中斷連線。</span><span class="sxs-lookup"><span data-stu-id="513e0-410">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="513e0-411">Empty</span><span class="sxs-lookup"><span data-stu-id="513e0-411">Empty</span></span> | <span data-ttu-id="513e0-412">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="513e0-412">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="513e0-413">委派，可以用來設定或取代用來傳送 HTTP 要求的 `HttpMessageHandler`。</span><span class="sxs-lookup"><span data-stu-id="513e0-413">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="513e0-414">不用於 WebSocket 連接。</span><span class="sxs-lookup"><span data-stu-id="513e0-414">Not used for WebSocket connections.</span></span> <span data-ttu-id="513e0-415">這個委派必須傳回非 null 值，而且會收到預設值做為參數。</span><span class="sxs-lookup"><span data-stu-id="513e0-415">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="513e0-416">請修改該預設值的設定並將其傳回，或傳回新的 `HttpMessageHandler` 實例。</span><span class="sxs-lookup"><span data-stu-id="513e0-416">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="513e0-417">**取代處理常式時，請務必從提供的處理常式中複製您想要保留的設定，否則設定的選項（例如 Cookie 和標頭）將不會套用至新的處理常式。**</span><span class="sxs-lookup"><span data-stu-id="513e0-417">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="513e0-418">傳送 HTTP 要求時所要使用的 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="513e0-418">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="513e0-419">設定此布林值，以傳送 HTTP 和 Websocket 要求的預設認證。</span><span class="sxs-lookup"><span data-stu-id="513e0-419">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="513e0-420">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="513e0-420">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="513e0-421">可以用來設定其他 WebSocket 選項的委派。</span><span class="sxs-lookup"><span data-stu-id="513e0-421">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="513e0-422">接收可用於設定選項的[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)實例。</span><span class="sxs-lookup"><span data-stu-id="513e0-422">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="513e0-423">JavaScript</span><span class="sxs-lookup"><span data-stu-id="513e0-423">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="513e0-424">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="513e0-424">JavaScript Option</span></span> | <span data-ttu-id="513e0-425">Default Value</span><span class="sxs-lookup"><span data-stu-id="513e0-425">Default Value</span></span> | <span data-ttu-id="513e0-426">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-426">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="513e0-427">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="513e0-427">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="513e0-428">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="513e0-428">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="513e0-429">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="513e0-429">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="513e0-430">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-430">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="513e0-431">Java</span><span class="sxs-lookup"><span data-stu-id="513e0-431">Java</span></span>](#tab/java)

| <span data-ttu-id="513e0-432">JAVA 選項</span><span class="sxs-lookup"><span data-stu-id="513e0-432">Java Option</span></span> | <span data-ttu-id="513e0-433">Default Value</span><span class="sxs-lookup"><span data-stu-id="513e0-433">Default Value</span></span> | <span data-ttu-id="513e0-434">描述</span><span class="sxs-lookup"><span data-stu-id="513e0-434">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="513e0-435">傳回字串的函式，在 HTTP 要求中提供為持有人驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="513e0-435">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="513e0-436">將此設為 `true` 以略過協商步驟。</span><span class="sxs-lookup"><span data-stu-id="513e0-436">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="513e0-437">**只有在 websocket 傳輸為唯一啟用的傳輸時才支援**。</span><span class="sxs-lookup"><span data-stu-id="513e0-437">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="513e0-438">使用 Azure SignalR Service 時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="513e0-438">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="513e0-439">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="513e0-439">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="513e0-440">Empty</span><span class="sxs-lookup"><span data-stu-id="513e0-440">Empty</span></span> | <span data-ttu-id="513e0-441">要隨每個 HTTP 要求傳送之其他 HTTP 標頭的對應。</span><span class="sxs-lookup"><span data-stu-id="513e0-441">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="513e0-442">在 .NET 用戶端中，這些選項可以由提供給 `WithUrl` 的選項委派進行修改：</span><span class="sxs-lookup"><span data-stu-id="513e0-442">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="513e0-443">在 JavaScript 用戶端中，可以在提供給 `withUrl` 的 JavaScript 物件中提供這些選項：</span><span class="sxs-lookup"><span data-stu-id="513e0-443">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="513e0-444">在 JAVA 用戶端中，可以使用從 `HubConnectionBuilder.create("HUB URL")` 傳回的 `HttpHubConnectionBuilder` 上的方法來設定這些選項。</span><span class="sxs-lookup"><span data-stu-id="513e0-444">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="513e0-445">其他資源</span><span class="sxs-lookup"><span data-stu-id="513e0-445">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
