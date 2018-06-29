---
title: SignalR 和 ASP.NET Core SignalR 之間的差異
author: rachelappel
description: SignalR 和 ASP.NET Core SignalR 之間的差異
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090059"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="ac609-103">SignalR 和 ASP.NET Core SignalR 之間的差異</span><span class="sxs-lookup"><span data-stu-id="ac609-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="ac609-104">ASP.NET Core SignalR 與不相容用戶端或適用於 ASP.NET SignalR 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="ac609-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="ac609-105">這篇文章說明已移除或變更 ASP.NET Core SignalR 中的功能。</span><span class="sxs-lookup"><span data-stu-id="ac609-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="ac609-106">功能差異</span><span class="sxs-lookup"><span data-stu-id="ac609-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="ac609-107">自動重新</span><span class="sxs-lookup"><span data-stu-id="ac609-107">Automatic reconnects</span></span>

<span data-ttu-id="ac609-108">不再支援自動重新。</span><span class="sxs-lookup"><span data-stu-id="ac609-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="ac609-109">先前，SignalR 會嘗試重新連線到伺服器，如果連線已中斷。</span><span class="sxs-lookup"><span data-stu-id="ac609-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="ac609-110">現在，如果用戶端中斷連線的使用者必須明確啟動新的連接，如果他們想要重新連線。</span><span class="sxs-lookup"><span data-stu-id="ac609-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="ac609-111">通訊協定支援</span><span class="sxs-lookup"><span data-stu-id="ac609-111">Protocol support</span></span>

<span data-ttu-id="ac609-112">ASP.NET Core SignalR 支援 JSON，以及新的二進位通訊協定，以根據[MessagePack](xref:signalr/messagepackhubprotocol)。</span><span class="sxs-lookup"><span data-stu-id="ac609-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="ac609-113">此外，您可以建立自訂通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ac609-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="ac609-114">在伺服器上的差異</span><span class="sxs-lookup"><span data-stu-id="ac609-114">Differences on the server</span></span>

<span data-ttu-id="ac609-115">SignalR 的伺服器端程式庫隨附的`Microsoft.AspNetCore.App`所屬的封裝**ASP.NET Core Web 應用程式**Razor 和 MVC 專案範本。</span><span class="sxs-lookup"><span data-stu-id="ac609-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="ac609-116">SignalR 是 ASP.NET Core 中介軟體，因此它必須藉由呼叫設定`AddSignalR`中`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="ac609-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="ac609-117">若要設定路由，請將路由對應至中樞內`UseSignalR`方法呼叫中`Startup.Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="ac609-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="ac609-118">現在必須要有的自黏工作階段</span><span class="sxs-lookup"><span data-stu-id="ac609-118">Sticky sessions now required</span></span>

<span data-ttu-id="ac609-119">如何向外延展使用過舊版 SignalR 的因為用戶端無法重新連線，並將訊息傳送至伺服器陣列中的任何伺服器。</span><span class="sxs-lookup"><span data-stu-id="ac609-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="ac609-120">由於在向外延展模型，以及不支援重新變更，這已不再支援。</span><span class="sxs-lookup"><span data-stu-id="ac609-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="ac609-121">用戶端連線到伺服器之後必須立即互動同一部伺服器用於連接持續時間。</span><span class="sxs-lookup"><span data-stu-id="ac609-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="ac609-122">針對每個連接的單一中樞</span><span class="sxs-lookup"><span data-stu-id="ac609-122">Single hub per connection</span></span>

<span data-ttu-id="ac609-123">在 ASP.NET Core SignalR，已經過簡化的連接模式。</span><span class="sxs-lookup"><span data-stu-id="ac609-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="ac609-124">連接的直接單一中樞，而不是單一連線正在使用共用存取多個中樞。</span><span class="sxs-lookup"><span data-stu-id="ac609-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="ac609-125">資料流</span><span class="sxs-lookup"><span data-stu-id="ac609-125">Streaming</span></span>

<span data-ttu-id="ac609-126">SignalR 現在支援[串流資料](xref:signalr/streaming)從用戶端的中樞。</span><span class="sxs-lookup"><span data-stu-id="ac609-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="ac609-127">狀況</span><span class="sxs-lookup"><span data-stu-id="ac609-127">State</span></span>

<span data-ttu-id="ac609-128">用戶端與中樞 （通常稱為 HubState） 之間傳遞任意的狀態的能力被移除了，以及支援進度訊息。</span><span class="sxs-lookup"><span data-stu-id="ac609-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="ac609-129">目前的中樞 proxy 沒有對應項目。</span><span class="sxs-lookup"><span data-stu-id="ac609-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="ac609-130">在用戶端上的差異</span><span class="sxs-lookup"><span data-stu-id="ac609-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="ac609-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="ac609-131">TypeScript</span></span>

<span data-ttu-id="ac609-132">SignalR 的 ASP.NET Core 版本撰寫的[TypeScript](https://www.typescriptlang.org/)。</span><span class="sxs-lookup"><span data-stu-id="ac609-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="ac609-133">使用時，您可以撰寫的 JavaScript 或 TypeScript [JavaScript 用戶端](xref:signalr/javascript-client)。</span><span class="sxs-lookup"><span data-stu-id="ac609-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="ac609-134">JavaScript 用戶端會裝載於[npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="ac609-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="ac609-135">在舊版中，JavaScript 用戶端已取得透過 Visual Studio 中的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="ac609-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="ac609-136">Core 版本中， [ @aspnet/signalr npm 封裝](https://www.npmjs.com/package/@aspnet/signalr)包含 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="ac609-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="ac609-137">此套件不包含在**ASP.NET Core Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="ac609-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="ac609-138">用於取得並安裝 npm `@aspnet/signalr` npm 封裝。</span><span class="sxs-lookup"><span data-stu-id="ac609-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="ac609-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="ac609-139">jQuery</span></span>

<span data-ttu-id="ac609-140">已移除對 jQuery 的相依性，但專案仍然可以使用 jQuery。</span><span class="sxs-lookup"><span data-stu-id="ac609-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="ac609-141">JavaScript 用戶端方法語法</span><span class="sxs-lookup"><span data-stu-id="ac609-141">JavaScript client method syntax</span></span>

<span data-ttu-id="ac609-142">JavaScript 語法已從舊版 SignalR 的變更。</span><span class="sxs-lookup"><span data-stu-id="ac609-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="ac609-143">而不是使用`$connection`物件，請建立連線，使用`HubConnectionBuilder`應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="ac609-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="ac609-144">使用`connection.on`指定中樞可以呼叫的用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="ac609-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="ac609-145">建立用戶端方法之後, 啟動中樞連線。</span><span class="sxs-lookup"><span data-stu-id="ac609-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="ac609-146">鏈結`catch`方法，以記錄或處理錯誤。</span><span class="sxs-lookup"><span data-stu-id="ac609-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="ac609-147">中樞 proxy</span><span class="sxs-lookup"><span data-stu-id="ac609-147">Hub proxies</span></span>

<span data-ttu-id="ac609-148">不會再自動產生中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="ac609-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="ac609-149">相反地，傳送到的方法名稱`invoke`API 為字串。</span><span class="sxs-lookup"><span data-stu-id="ac609-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="ac609-150">.NET 和其他用戶端</span><span class="sxs-lookup"><span data-stu-id="ac609-150">.NET and other clients</span></span>

<span data-ttu-id="ac609-151">`Microsoft.AspNetCore.SignalR.Client` NuGet 套件包含適用於 ASP.NET Core SignalR 的.NET 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="ac609-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="ac609-152">使用`HubConnectionBuilder`以建立並建置連線至中樞的執行個體。</span><span class="sxs-lookup"><span data-stu-id="ac609-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="ac609-153">其他資源</span><span class="sxs-lookup"><span data-stu-id="ac609-153">Additional resources</span></span>

* [<span data-ttu-id="ac609-154">中樞</span><span class="sxs-lookup"><span data-stu-id="ac609-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ac609-155">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="ac609-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ac609-156">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="ac609-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ac609-157">支援的平台</span><span class="sxs-lookup"><span data-stu-id="ac609-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
