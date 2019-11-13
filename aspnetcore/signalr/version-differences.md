---
title: SignalR 和 ASP.NET Core 之間的差異 SignalR
author: bradygaster
description: SignalR 和 ASP.NET Core 之間的差異 SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: 0f644c132b0fcf9a0ecf0ab181791a6477c97f76
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963727"
---
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a><span data-ttu-id="f6580-103">ASP.NET SignalR 與 ASP.NET Core 之間的差異 SignalR</span><span class="sxs-lookup"><span data-stu-id="f6580-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="f6580-104">ASP.NET Core SignalR 與 ASP.NET SignalR的用戶端或伺服器不相容。</span><span class="sxs-lookup"><span data-stu-id="f6580-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="f6580-105">本文詳細說明在 ASP.NET Core SignalR中已移除或變更的功能。</span><span class="sxs-lookup"><span data-stu-id="f6580-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-opno-locsignalr-version"></a><span data-ttu-id="f6580-106">如何識別 SignalR 版本</span><span class="sxs-lookup"><span data-stu-id="f6580-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="f6580-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="f6580-107">ASP.NET SignalR</span></span> | <span data-ttu-id="f6580-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="f6580-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="f6580-109">伺服器 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="f6580-109">Server NuGet Package</span></span> | <span data-ttu-id="f6580-110">[SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="f6580-110">[Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span></span> | <span data-ttu-id="f6580-111">[AspNetCore 應用程式](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)（.net Core）</span><span class="sxs-lookup"><span data-stu-id="f6580-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="f6580-112">[AspNetCore。SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="f6580-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span></span> <span data-ttu-id="f6580-113">（.NET Framework）</span><span class="sxs-lookup"><span data-stu-id="f6580-113">(.NET Framework)</span></span> |
| <span data-ttu-id="f6580-114">用戶端 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="f6580-114">Client NuGet Packages</span></span> | <span data-ttu-id="f6580-115">[SignalR。台](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="f6580-115">[Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span></span><br><span data-ttu-id="f6580-116">[SignalR。NODE.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span><span class="sxs-lookup"><span data-stu-id="f6580-116">[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span></span> | <span data-ttu-id="f6580-117">[AspNetCore.SignalR。台](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="f6580-117">[Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span></span> |
| <span data-ttu-id="f6580-118">用戶端 npm 套件</span><span class="sxs-lookup"><span data-stu-id="f6580-118">Client npm Package</span></span> | [<span data-ttu-id="f6580-119">signalr</span><span class="sxs-lookup"><span data-stu-id="f6580-119">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="f6580-120">JAVA 用戶端</span><span class="sxs-lookup"><span data-stu-id="f6580-120">Java Client</span></span> | <span data-ttu-id="f6580-121">[GitHub 存放庫](https://github.com/SignalR/java-client)（已淘汰）</span><span class="sxs-lookup"><span data-stu-id="f6580-121">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="f6580-122">Maven package [com. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="f6580-122">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="f6580-123">伺服器應用程式類型</span><span class="sxs-lookup"><span data-stu-id="f6580-123">Server App Type</span></span> | <span data-ttu-id="f6580-124">ASP.NET （System.web）或 OWIN 自我裝載</span><span class="sxs-lookup"><span data-stu-id="f6580-124">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="f6580-125">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6580-125">ASP.NET Core</span></span> |
| <span data-ttu-id="f6580-126">支援的伺服器平臺</span><span class="sxs-lookup"><span data-stu-id="f6580-126">Supported Server Platforms</span></span> | <span data-ttu-id="f6580-127">.NET Framework 4.5 或更新版本</span><span class="sxs-lookup"><span data-stu-id="f6580-127">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="f6580-128">.NET Framework 4.6.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="f6580-128">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="f6580-129">.NET Core 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="f6580-129">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="f6580-130">功能差異</span><span class="sxs-lookup"><span data-stu-id="f6580-130">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="f6580-131">自動重新連接</span><span class="sxs-lookup"><span data-stu-id="f6580-131">Automatic reconnects</span></span>

<span data-ttu-id="f6580-132">ASP.NET Core SignalR不支援自動重新連接。</span><span class="sxs-lookup"><span data-stu-id="f6580-132">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="f6580-133">如果用戶端已中斷連線，則使用者必須明確地啟動新的連線（如果他們想要重新連接）。</span><span class="sxs-lookup"><span data-stu-id="f6580-133">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="f6580-134">在 ASP.NET SignalR中，如果中斷連接，SignalR 會嘗試重新連線到伺服器。</span><span class="sxs-lookup"><span data-stu-id="f6580-134">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="f6580-135">通訊協定支援</span><span class="sxs-lookup"><span data-stu-id="f6580-135">Protocol support</span></span>

<span data-ttu-id="f6580-136">ASP.NET Core SignalR 支援 JSON，以及以[MessagePack](xref:signalr/messagepackhubprotocol)為基礎的新二進位通訊協定。</span><span class="sxs-lookup"><span data-stu-id="f6580-136">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="f6580-137">此外，也可以建立自訂通訊協定。</span><span class="sxs-lookup"><span data-stu-id="f6580-137">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="f6580-138">傳輸</span><span class="sxs-lookup"><span data-stu-id="f6580-138">Transports</span></span>

<span data-ttu-id="f6580-139">ASP.NET Core SignalR中不支援永久的框架傳輸。</span><span class="sxs-lookup"><span data-stu-id="f6580-139">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="f6580-140">伺服器上的差異</span><span class="sxs-lookup"><span data-stu-id="f6580-140">Differences on the server</span></span>

<span data-ttu-id="f6580-141">ASP.NET Core SignalR 伺服器端程式庫包含在適用于 Razor 和 MVC 專案的**ASP.NET Core Web 應用程式**範本中的[中繼套件](xref:fundamentals/metapackage-app)套件。</span><span class="sxs-lookup"><span data-stu-id="f6580-141">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="f6580-142">ASP.NET Core SignalR 是 ASP.NET Core 中介軟體，所以必須藉由在 `Startup.ConfigureServices`中呼叫[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)來設定。</span><span class="sxs-lookup"><span data-stu-id="f6580-142">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f6580-143">若要設定路由，請在 `Startup.Configure` 方法中的[UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints)方法呼叫內，將路由對應至中樞。</span><span class="sxs-lookup"><span data-stu-id="f6580-143">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="f6580-144">若要設定路由，請在 `Startup.Configure` 方法中的[UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr)方法呼叫內，將路由對應至中樞。</span><span class="sxs-lookup"><span data-stu-id="f6580-144">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="f6580-145">粘滯話</span><span class="sxs-lookup"><span data-stu-id="f6580-145">Sticky sessions</span></span>

<span data-ttu-id="f6580-146">ASP.NET SignalR 的向外延展模型可讓用戶端重新連線，並將訊息傳送至伺服器陣列中的任何伺服器。</span><span class="sxs-lookup"><span data-stu-id="f6580-146">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="f6580-147">在 ASP.NET Core SignalR中，用戶端必須在連接期間與相同的伺服器互動。</span><span class="sxs-lookup"><span data-stu-id="f6580-147">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="f6580-148">針對使用 Redis 的向外延展，這表示需要有粘滯會話。</span><span class="sxs-lookup"><span data-stu-id="f6580-148">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="f6580-149">針對使用[Azure SignalR 服務](/azure/azure-signalr/)的向外延展，不需要粘滯會話，因為服務會處理用戶端的連線。</span><span class="sxs-lookup"><span data-stu-id="f6580-149">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="f6580-150">每個連接的單一中樞</span><span class="sxs-lookup"><span data-stu-id="f6580-150">Single hub per connection</span></span>

<span data-ttu-id="f6580-151">在 ASP.NET Core SignalR中，已簡化連接模型。</span><span class="sxs-lookup"><span data-stu-id="f6580-151">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="f6580-152">直接連接到單一中樞，而不是使用單一連線來共用多個中樞的存取權。</span><span class="sxs-lookup"><span data-stu-id="f6580-152">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="f6580-153">資料流</span><span class="sxs-lookup"><span data-stu-id="f6580-153">Streaming</span></span>

<span data-ttu-id="f6580-154">ASP.NET Core SignalR 現在支援從中樞將[資料串流](xref:signalr/streaming)至用戶端。</span><span class="sxs-lookup"><span data-stu-id="f6580-154">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="f6580-155">狀況</span><span class="sxs-lookup"><span data-stu-id="f6580-155">State</span></span>

<span data-ttu-id="f6580-156">在用戶端與中樞之間傳遞任意狀態的能力（通常稱為 HubState）已移除，並支援進度訊息。</span><span class="sxs-lookup"><span data-stu-id="f6580-156">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="f6580-157">目前沒有任何對應的中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="f6580-157">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="f6580-158">PersistentConnection 移除</span><span class="sxs-lookup"><span data-stu-id="f6580-158">PersistentConnection removal</span></span>

<span data-ttu-id="f6580-159">在 ASP.NET Core SignalR中，已移除[PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118))類別。</span><span class="sxs-lookup"><span data-stu-id="f6580-159">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="f6580-160">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="f6580-160">GlobalHost</span></span>

<span data-ttu-id="f6580-161">ASP.NET Core 在架構內建相依性插入（DI）。</span><span class="sxs-lookup"><span data-stu-id="f6580-161">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="f6580-162">服務可以使用 DI 來存取[HubCoNtext](xref:signalr/hubcontext)。</span><span class="sxs-lookup"><span data-stu-id="f6580-162">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="f6580-163">ASP.NET SignalR 中用來取得 `HubContext` 的 `GlobalHost` 物件不存在於 ASP.NET Core SignalR中。</span><span class="sxs-lookup"><span data-stu-id="f6580-163">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="f6580-164">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="f6580-164">HubPipeline</span></span>

<span data-ttu-id="f6580-165">ASP.NET Core SignalR 不支援 `HubPipeline` 模組。</span><span class="sxs-lookup"><span data-stu-id="f6580-165">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="f6580-166">用戶端上的差異</span><span class="sxs-lookup"><span data-stu-id="f6580-166">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="f6580-167">TypeScript</span><span class="sxs-lookup"><span data-stu-id="f6580-167">TypeScript</span></span>

<span data-ttu-id="f6580-168">ASP.NET Core SignalR 用戶端是以[TypeScript](https://www.typescriptlang.org/)撰寫。</span><span class="sxs-lookup"><span data-stu-id="f6580-168">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="f6580-169">使用[javascript 用戶端](xref:signalr/javascript-client)時，您可以使用 JAVAscript 或 TypeScript 來撰寫。</span><span class="sxs-lookup"><span data-stu-id="f6580-169">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="f6580-170">JavaScript 用戶端裝載于[npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="f6580-170">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="f6580-171">在先前的版本中，JavaScript 用戶端是透過 Visual Studio 中的 NuGet 套件取得。</span><span class="sxs-lookup"><span data-stu-id="f6580-171">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="f6580-172">針對核心版本， [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm 套件包含 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="f6580-172">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="f6580-173">此套件不包含在**ASP.NET Core Web 應用程式**範本中。</span><span class="sxs-lookup"><span data-stu-id="f6580-173">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="f6580-174">使用 npm 取得並安裝 `@aspnet/signalr` npm 套件。</span><span class="sxs-lookup"><span data-stu-id="f6580-174">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="f6580-175">jQuery</span><span class="sxs-lookup"><span data-stu-id="f6580-175">jQuery</span></span>

<span data-ttu-id="f6580-176">已移除 jQuery 的相依性，不過專案仍然可以使用 jQuery。</span><span class="sxs-lookup"><span data-stu-id="f6580-176">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="f6580-177">Internet Explorer 支援</span><span class="sxs-lookup"><span data-stu-id="f6580-177">Internet Explorer support</span></span>

<span data-ttu-id="f6580-178">ASP.NET Core SignalR 需要 Microsoft Internet Explorer 11 或更新版本（ASP.NET SignalR 支援 Microsoft Internet Explorer 8 和更新版本）。</span><span class="sxs-lookup"><span data-stu-id="f6580-178">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="f6580-179">JavaScript 用戶端方法語法</span><span class="sxs-lookup"><span data-stu-id="f6580-179">JavaScript client method syntax</span></span>

<span data-ttu-id="f6580-180">JavaScript 語法已從舊版的 SignalR變更。</span><span class="sxs-lookup"><span data-stu-id="f6580-180">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="f6580-181">您不需要使用 `$connection` 物件，而是使用[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API 建立連接。</span><span class="sxs-lookup"><span data-stu-id="f6580-181">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="f6580-182">使用[on](/javascript/api/@aspnet/signalr/HubConnection#on)方法來指定中樞可以呼叫的用戶端方法。</span><span class="sxs-lookup"><span data-stu-id="f6580-182">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="f6580-183">建立用戶端方法之後，請啟動中樞連接。</span><span class="sxs-lookup"><span data-stu-id="f6580-183">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="f6580-184">建立[catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)方法的鏈，以記錄或處理錯誤。</span><span class="sxs-lookup"><span data-stu-id="f6580-184">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="f6580-185">中樞 proxy</span><span class="sxs-lookup"><span data-stu-id="f6580-185">Hub proxies</span></span>

<span data-ttu-id="f6580-186">不會再自動產生中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="f6580-186">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="f6580-187">相反地，方法名稱會以字串形式傳遞至[invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API。</span><span class="sxs-lookup"><span data-stu-id="f6580-187">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="f6580-188">.NET 和其他用戶端</span><span class="sxs-lookup"><span data-stu-id="f6580-188">.NET and other clients</span></span>

<span data-ttu-id="f6580-189">`Microsoft.AspNetCore.SignalR.Client` NuGet 套件包含適用于 ASP.NET Core SignalR的 .NET 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="f6580-189">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="f6580-190">使用[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)來建立和建立與中樞的連接實例。</span><span class="sxs-lookup"><span data-stu-id="f6580-190">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="f6580-191">向外延展差異</span><span class="sxs-lookup"><span data-stu-id="f6580-191">Scaleout differences</span></span>

<span data-ttu-id="f6580-192">ASP.NET SignalR 支援 SQL Server 和 Redis。</span><span class="sxs-lookup"><span data-stu-id="f6580-192">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="f6580-193">ASP.NET Core SignalR 支援 Azure SignalR 服務和 Redis。</span><span class="sxs-lookup"><span data-stu-id="f6580-193">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="f6580-194">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f6580-194">ASP.NET</span></span>

* <span data-ttu-id="f6580-195">[使用 Azure 服務匯流排 SignalR 向外延展](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span><span class="sxs-lookup"><span data-stu-id="f6580-195">[SignalR scaleout with Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span></span>
* <span data-ttu-id="f6580-196">[使用 Redis SignalR 向外延展](/aspnet/signalr/overview/performance/scaleout-with-redis)</span><span class="sxs-lookup"><span data-stu-id="f6580-196">[SignalR scaleout with Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span></span>
* <span data-ttu-id="f6580-197">[使用 SQL Server SignalR 向外延展](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span><span class="sxs-lookup"><span data-stu-id="f6580-197">[SignalR scaleout with SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span></span>

### <a name="aspnet-core"></a><span data-ttu-id="f6580-198">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6580-198">ASP.NET Core</span></span>

* <span data-ttu-id="f6580-199">[Azure SignalR 服務](/azure/azure-signalr/)</span><span class="sxs-lookup"><span data-stu-id="f6580-199">[Azure SignalR Service](/azure/azure-signalr/)</span></span>
* [<span data-ttu-id="f6580-200">Redis 背板</span><span class="sxs-lookup"><span data-stu-id="f6580-200">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="f6580-201">其他資源</span><span class="sxs-lookup"><span data-stu-id="f6580-201">Additional resources</span></span>

* [<span data-ttu-id="f6580-202">中樞</span><span class="sxs-lookup"><span data-stu-id="f6580-202">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f6580-203">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="f6580-203">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="f6580-204">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="f6580-204">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="f6580-205">支援的平台</span><span class="sxs-lookup"><span data-stu-id="f6580-205">Supported platforms</span></span>](xref:signalr/supported-platforms)
