---
title: SignalR 和 ASP.NET Core SignalR 之間的差異
author: tdykstra
description: SignalR 和 ASP.NET Core SignalR 之間的差異
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: ea2de2606a99de70fa645c0c42303525fea0a44e
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325532"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="c7ab5-103">ASP.NET SignalR 及 ASP.NET Core SignalR 之間的差異</span><span class="sxs-lookup"><span data-stu-id="c7ab5-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="c7ab5-104">ASP.NET Core SignalR 與不相容用戶端或為 ASP.NET SignalR 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="c7ab5-105">本文詳細說明已經被移除，或在 ASP.NET Core SignalR 中變更的功能。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="c7ab5-106">如何找出 SignalR 版本</span><span class="sxs-lookup"><span data-stu-id="c7ab5-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="c7ab5-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="c7ab5-107">ASP.NET SignalR</span></span> | <span data-ttu-id="c7ab5-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c7ab5-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="c7ab5-109">伺服器 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="c7ab5-109">Server NuGet Package</span></span> | [<span data-ttu-id="c7ab5-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="c7ab5-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="c7ab5-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="c7ab5-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="c7ab5-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="c7ab5-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="c7ab5-113">用戶端 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="c7ab5-113">Client NuGet Packages</span></span> | [<span data-ttu-id="c7ab5-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="c7ab5-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="c7ab5-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="c7ab5-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="c7ab5-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="c7ab5-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="c7ab5-117">用戶端 npm 套件</span><span class="sxs-lookup"><span data-stu-id="c7ab5-117">Client npm Package</span></span> | [<span data-ttu-id="c7ab5-118">signalr</span><span class="sxs-lookup"><span data-stu-id="c7ab5-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="c7ab5-119">伺服器應用程式類型</span><span class="sxs-lookup"><span data-stu-id="c7ab5-119">Server App Type</span></span> | <span data-ttu-id="c7ab5-120">ASP.NET (System.Web) 或 OWIN 自我裝載</span><span class="sxs-lookup"><span data-stu-id="c7ab5-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="c7ab5-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7ab5-121">ASP.NET Core</span></span> |
| <span data-ttu-id="c7ab5-122">支援的伺服器平台</span><span class="sxs-lookup"><span data-stu-id="c7ab5-122">Supported Server Platforms</span></span> | <span data-ttu-id="c7ab5-123">.NET framework 4.5 或更新版本</span><span class="sxs-lookup"><span data-stu-id="c7ab5-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="c7ab5-124">.NET Framework 4.6.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="c7ab5-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="c7ab5-125">.NET core 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="c7ab5-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="c7ab5-126">功能差異</span><span class="sxs-lookup"><span data-stu-id="c7ab5-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="c7ab5-127">自動重新連線</span><span class="sxs-lookup"><span data-stu-id="c7ab5-127">Automatic reconnects</span></span>

<span data-ttu-id="c7ab5-128">ASP.NET Core SignalR 並不支援自動重新連線。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="c7ab5-129">如果用戶端已中斷連接，使用者必須明確啟動新的連線如果他們想要重新連線。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="c7ab5-130">在 ASP.NET SignalR、 SignalR 會嘗試重新連線到伺服器，如果連接已卸除。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="c7ab5-131">通訊協定支援</span><span class="sxs-lookup"><span data-stu-id="c7ab5-131">Protocol support</span></span>

<span data-ttu-id="c7ab5-132">ASP.NET Core SignalR 支援 JSON，以及新的二進位通訊協定，以根據[MessagePack](xref:signalr/messagepackhubprotocol)。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="c7ab5-133">此外，您可以建立自訂通訊協定。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="c7ab5-134">在伺服器上的差異</span><span class="sxs-lookup"><span data-stu-id="c7ab5-134">Differences on the server</span></span>

<span data-ttu-id="c7ab5-135">ASP.NET Core SignalR 的伺服器端程式庫都會納入[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)封裝的一部分**ASP.NET Core Web 應用程式**Razor 和 MVC 範本專案。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="c7ab5-136">ASP.NET Core SignalR 是 ASP.NET Core 中介軟體，因此必須由呼叫[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)在`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="c7ab5-137">若要設定路由，請將路由對應至中樞內[UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr)方法呼叫中`Startup.Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="c7ab5-138">現在需要黏性工作階段</span><span class="sxs-lookup"><span data-stu-id="c7ab5-138">Sticky sessions now required</span></span>

<span data-ttu-id="c7ab5-139">如何向外延展中使用過 ASP.NET SignalR，因為用戶端無法重新連線，並將訊息傳送至伺服器陣列中的任何伺服器。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-139">Because of how scale-out worked in ASP.NET SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="c7ab5-140">由於變更向外延展模型，以及不支援重新連線，因此這是不受支援。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-140">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="c7ab5-141">一旦用戶端連接到伺服器，它必須與相同的伺服器互動期間的連線。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-141">Once the client connects to the server, it must interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="c7ab5-142">每個連線單一中樞</span><span class="sxs-lookup"><span data-stu-id="c7ab5-142">Single hub per connection</span></span>

<span data-ttu-id="c7ab5-143">在 ASP.NET Core SignalR 連線模型已簡化。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-143">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="c7ab5-144">連線會對直接單一中樞中，而不是正在使用共用存取權的多個中樞單一連接。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-144">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="c7ab5-145">資料流</span><span class="sxs-lookup"><span data-stu-id="c7ab5-145">Streaming</span></span>

<span data-ttu-id="c7ab5-146">ASP.NET Core SignalR 現在支援[串流資料](xref:signalr/streaming)從用戶端中樞。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-146">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="c7ab5-147">狀況</span><span class="sxs-lookup"><span data-stu-id="c7ab5-147">State</span></span>

<span data-ttu-id="c7ab5-148">能夠將任意的狀態傳遞用戶端與中樞 （通常稱為 HubState） 之間已移除，以及支援進度訊息。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-148">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="c7ab5-149">目前沒有任何對應的中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-149">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="c7ab5-150">在用戶端上的差異</span><span class="sxs-lookup"><span data-stu-id="c7ab5-150">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="c7ab5-151">TypeScript</span><span class="sxs-lookup"><span data-stu-id="c7ab5-151">TypeScript</span></span>

<span data-ttu-id="c7ab5-152">ASP.NET Core SignalR 用戶端以[TypeScript](https://www.typescriptlang.org/)。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-152">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="c7ab5-153">使用時，您可以撰寫以 JavaScript 或 TypeScript [JavaScript 用戶端](xref:signalr/javascript-client)。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-153">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="c7ab5-154">JavaScript 用戶端裝載於[npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="c7ab5-154">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="c7ab5-155">在舊版中，JavaScript 用戶端已取得透過 Visual Studio 中的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-155">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="c7ab5-156">如需核心版本中， [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) npm 套件所包含的 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-156">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="c7ab5-157">此套件不會包含於**ASP.NET Core Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-157">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="c7ab5-158">使用 npm 來取得並安裝`@aspnet/signalr`npm 套件。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-158">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="c7ab5-159">jQuery</span><span class="sxs-lookup"><span data-stu-id="c7ab5-159">jQuery</span></span>

<span data-ttu-id="c7ab5-160">已移除對 jQuery 的相依性，但專案仍然可以使用 jQuery。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-160">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="c7ab5-161">JavaScript 用戶端方法語法</span><span class="sxs-lookup"><span data-stu-id="c7ab5-161">JavaScript client method syntax</span></span>

<span data-ttu-id="c7ab5-162">JavaScript 語法已從舊版 SignalR 的變更。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-162">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="c7ab5-163">而不是使用`$connection`物件，請建立連線，使用[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-163">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="c7ab5-164">使用[上](/javascript/api/@aspnet/signalr/HubConnection#on)方法，以指定用戶端中樞可以呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-164">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="c7ab5-165">建立用戶端方法之後, 啟動中樞連線。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-165">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="c7ab5-166">鏈結[攔截](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)記錄或處理錯誤的方法。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-166">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="c7ab5-167">中樞 proxy</span><span class="sxs-lookup"><span data-stu-id="c7ab5-167">Hub proxies</span></span>

<span data-ttu-id="c7ab5-168">中樞 proxy 不會再自動產生。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-168">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="c7ab5-169">相反地，在方法名稱會傳遞至[叫用](/javascript/api/%40aspnet/signalr/hubconnection#invoke)API 做為字串。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-169">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="c7ab5-170">.NET 和其他用戶端</span><span class="sxs-lookup"><span data-stu-id="c7ab5-170">.NET and other clients</span></span>

<span data-ttu-id="c7ab5-171">`Microsoft.AspNetCore.SignalR.Client` NuGet 套件包含.NET 用戶端程式庫的 ASP.NET Core SignalR。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-171">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="c7ab5-172">使用[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)來建立及建置連接到中樞執行個體。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-172">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="c7ab5-173">向外延展的差異</span><span class="sxs-lookup"><span data-stu-id="c7ab5-173">Scaleout differences</span></span>

<span data-ttu-id="c7ab5-174">ASP.NET SignalR 支援 SQL Server 和 Redis。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-174">ASP.NET SignalR supports both SQL Server and Redis.</span></span> <span data-ttu-id="c7ab5-175">ASP.NET Core SignalR 支援 Azure SignalR 服務和 Redis。</span><span class="sxs-lookup"><span data-stu-id="c7ab5-175">ASP.NET Core SignalR supports both Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="c7ab5-176">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c7ab5-176">ASP.NET</span></span>

* [<span data-ttu-id="c7ab5-177">SignalR 向外延展與 Azure 服務匯流排</span><span class="sxs-lookup"><span data-stu-id="c7ab5-177">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="c7ab5-178">使用 Redis 的 SignalR 向外延展</span><span class="sxs-lookup"><span data-stu-id="c7ab5-178">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="c7ab5-179">SignalR 向外延展與 SQL Server</span><span class="sxs-lookup"><span data-stu-id="c7ab5-179">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="c7ab5-180">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7ab5-180">ASP.NET Core</span></span>

* [<span data-ttu-id="c7ab5-181">Azure SignalR 服務</span><span class="sxs-lookup"><span data-stu-id="c7ab5-181">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="c7ab5-182">其他資源</span><span class="sxs-lookup"><span data-stu-id="c7ab5-182">Additional resources</span></span>

* [<span data-ttu-id="c7ab5-183">中樞</span><span class="sxs-lookup"><span data-stu-id="c7ab5-183">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c7ab5-184">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="c7ab5-184">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c7ab5-185">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="c7ab5-185">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c7ab5-186">支援的平台</span><span class="sxs-lookup"><span data-stu-id="c7ab5-186">Supported platforms</span></span>](xref:signalr/supported-platforms)
