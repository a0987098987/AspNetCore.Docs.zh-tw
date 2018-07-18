---
title: SignalR 和 ASP.NET Core SignalR 之間的差異
author: tdykstra
description: SignalR 和 ASP.NET Core SignalR 之間的差異
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095004"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="fee9c-103">SignalR 和 ASP.NET Core SignalR 之間的差異</span><span class="sxs-lookup"><span data-stu-id="fee9c-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="fee9c-104">ASP.NET Core SignalR 與不相容用戶端或為 ASP.NET SignalR 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="fee9c-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="fee9c-105">本文詳細說明已經被移除，或在 ASP.NET Core SignalR 中變更的功能。</span><span class="sxs-lookup"><span data-stu-id="fee9c-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="fee9c-106">功能差異</span><span class="sxs-lookup"><span data-stu-id="fee9c-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="fee9c-107">自動重新連線</span><span class="sxs-lookup"><span data-stu-id="fee9c-107">Automatic reconnects</span></span>

<span data-ttu-id="fee9c-108">不再支援自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="fee9c-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="fee9c-109">先前，SignalR 會嘗試重新連線到伺服器，如果連線已中斷。</span><span class="sxs-lookup"><span data-stu-id="fee9c-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="fee9c-110">現在，如果用戶端中斷連線的使用者必須明確啟動新的連接，如果他們想要重新連線。</span><span class="sxs-lookup"><span data-stu-id="fee9c-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="fee9c-111">通訊協定支援</span><span class="sxs-lookup"><span data-stu-id="fee9c-111">Protocol support</span></span>

<span data-ttu-id="fee9c-112">ASP.NET Core SignalR 支援 JSON，以及新的二進位通訊協定，以根據[MessagePack](xref:signalr/messagepackhubprotocol)。</span><span class="sxs-lookup"><span data-stu-id="fee9c-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="fee9c-113">此外，您可以建立自訂通訊協定。</span><span class="sxs-lookup"><span data-stu-id="fee9c-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="fee9c-114">在伺服器上的差異</span><span class="sxs-lookup"><span data-stu-id="fee9c-114">Differences on the server</span></span>

<span data-ttu-id="fee9c-115">SignalR 的伺服器端程式庫都會納入`Microsoft.AspNetCore.App`封裝的一部分**ASP.NET Core Web 應用程式**Razor，MVC 專案範本。</span><span class="sxs-lookup"><span data-stu-id="fee9c-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="fee9c-116">SignalR 是 ASP.NET Core 中介軟體，因此必須由呼叫`AddSignalR`在`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="fee9c-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="fee9c-117">若要設定路由，請將路由對應至中樞內`UseSignalR`方法呼叫中`Startup.Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="fee9c-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="fee9c-118">現在需要黏性工作階段</span><span class="sxs-lookup"><span data-stu-id="fee9c-118">Sticky sessions now required</span></span>

<span data-ttu-id="fee9c-119">如何向外延展中使用過舊版 SignalR 的因為用戶端無法重新連線，並將訊息傳送至伺服器陣列中的任何伺服器。</span><span class="sxs-lookup"><span data-stu-id="fee9c-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="fee9c-120">由於變更向外延展模型，以及不支援重新連線，因此這是不受支援。</span><span class="sxs-lookup"><span data-stu-id="fee9c-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="fee9c-121">現在，一旦用戶端連接到伺服器它需要使用相同的伺服器在連線期間的互動方式。</span><span class="sxs-lookup"><span data-stu-id="fee9c-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="fee9c-122">每個連線單一中樞</span><span class="sxs-lookup"><span data-stu-id="fee9c-122">Single hub per connection</span></span>

<span data-ttu-id="fee9c-123">在 ASP.NET Core SignalR 連線模型已簡化。</span><span class="sxs-lookup"><span data-stu-id="fee9c-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="fee9c-124">連線會對直接單一中樞中，而不是正在使用共用存取權的多個中樞單一連接。</span><span class="sxs-lookup"><span data-stu-id="fee9c-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="fee9c-125">資料流</span><span class="sxs-lookup"><span data-stu-id="fee9c-125">Streaming</span></span>

<span data-ttu-id="fee9c-126">SignalR 現在支援[串流資料](xref:signalr/streaming)從用戶端中樞。</span><span class="sxs-lookup"><span data-stu-id="fee9c-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="fee9c-127">狀況</span><span class="sxs-lookup"><span data-stu-id="fee9c-127">State</span></span>

<span data-ttu-id="fee9c-128">能夠將任意的狀態傳遞用戶端與中樞 （通常稱為 HubState） 之間已移除，以及支援進度訊息。</span><span class="sxs-lookup"><span data-stu-id="fee9c-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="fee9c-129">目前沒有任何對應的中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="fee9c-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="fee9c-130">在用戶端上的差異</span><span class="sxs-lookup"><span data-stu-id="fee9c-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="fee9c-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="fee9c-131">TypeScript</span></span>

<span data-ttu-id="fee9c-132">SignalR 的 ASP.NET Core 版本以[TypeScript](https://www.typescriptlang.org/)。</span><span class="sxs-lookup"><span data-stu-id="fee9c-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="fee9c-133">使用時，您可以撰寫以 JavaScript 或 TypeScript [JavaScript 用戶端](xref:signalr/javascript-client)。</span><span class="sxs-lookup"><span data-stu-id="fee9c-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="fee9c-134">JavaScript 用戶端裝載於[npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="fee9c-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="fee9c-135">在舊版中，JavaScript 用戶端已取得透過 Visual Studio 中的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="fee9c-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="fee9c-136">如需核心版本中， [ @aspnet/signalr npm 套件](https://www.npmjs.com/package/@aspnet/signalr)包含 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="fee9c-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="fee9c-137">此套件不會包含於**ASP.NET Core Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="fee9c-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="fee9c-138">使用 npm 來取得並安裝`@aspnet/signalr`npm 套件。</span><span class="sxs-lookup"><span data-stu-id="fee9c-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="fee9c-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="fee9c-139">jQuery</span></span>

<span data-ttu-id="fee9c-140">已移除對 jQuery 的相依性，但專案仍然可以使用 jQuery。</span><span class="sxs-lookup"><span data-stu-id="fee9c-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="fee9c-141">JavaScript 用戶端方法語法</span><span class="sxs-lookup"><span data-stu-id="fee9c-141">JavaScript client method syntax</span></span>

<span data-ttu-id="fee9c-142">JavaScript 語法已從舊版 SignalR 的變更。</span><span class="sxs-lookup"><span data-stu-id="fee9c-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="fee9c-143">而不是使用`$connection`物件，請建立連線，使用`HubConnectionBuilder`API。</span><span class="sxs-lookup"><span data-stu-id="fee9c-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="fee9c-144">使用`connection.on`指定中樞可以呼叫的用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="fee9c-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="fee9c-145">建立用戶端方法之後, 啟動中樞連線。</span><span class="sxs-lookup"><span data-stu-id="fee9c-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="fee9c-146">鏈結`catch`記錄或處理錯誤的方法。</span><span class="sxs-lookup"><span data-stu-id="fee9c-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="fee9c-147">中樞 proxy</span><span class="sxs-lookup"><span data-stu-id="fee9c-147">Hub proxies</span></span>

<span data-ttu-id="fee9c-148">中樞 proxy 不會再自動產生。</span><span class="sxs-lookup"><span data-stu-id="fee9c-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="fee9c-149">相反地，傳送到的方法名稱`invoke`API 做為字串。</span><span class="sxs-lookup"><span data-stu-id="fee9c-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="fee9c-150">.NET 和其他用戶端</span><span class="sxs-lookup"><span data-stu-id="fee9c-150">.NET and other clients</span></span>

<span data-ttu-id="fee9c-151">`Microsoft.AspNetCore.SignalR.Client` NuGet 套件包含.NET 用戶端程式庫的 ASP.NET Core SignalR。</span><span class="sxs-lookup"><span data-stu-id="fee9c-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="fee9c-152">使用`HubConnectionBuilder`來建立及建置連接到中樞執行個體。</span><span class="sxs-lookup"><span data-stu-id="fee9c-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="fee9c-153">其他資源</span><span class="sxs-lookup"><span data-stu-id="fee9c-153">Additional resources</span></span>

* [<span data-ttu-id="fee9c-154">中樞</span><span class="sxs-lookup"><span data-stu-id="fee9c-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="fee9c-155">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="fee9c-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="fee9c-156">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="fee9c-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="fee9c-157">支援的平台</span><span class="sxs-lookup"><span data-stu-id="fee9c-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
