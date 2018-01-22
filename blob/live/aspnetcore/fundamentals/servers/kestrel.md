---
title: "ASP.NET Core kestrel web 伺服器實作"
author: tdykstra
description: "ASP.NET Core 根據 libuv 介紹 Kestrel，跨平台 web 伺服器。"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3695a6a127f77bd90538d72af6112ccf507f3482
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="045e5-103">ASP.NET Core Kestrel web 伺服器實作的簡介</span><span class="sxs-lookup"><span data-stu-id="045e5-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="045e5-104">由[Tom Dykstra](https://github.com/tdykstra)， [Chris Ross](https://github.com/Tratcher)，和[Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="045e5-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="045e5-105">Kestrel 是跨平台[適用於 ASP.NET Core web 伺服器](index.md)根據[libuv](https://github.com/libuv/libuv)，跨平台非同步 I/O 文件庫。</span><span class="sxs-lookup"><span data-stu-id="045e5-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="045e5-106">Kestrel 是 ASP.NET Core 專案範本中的預設隨附的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="045e5-106">Kestrel is the web server that is included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="045e5-107">Kestrel 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="045e5-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="045e5-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="045e5-108">HTTPS</span></span>
  * <span data-ttu-id="045e5-109">用來啟用不透明升級[WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="045e5-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="045e5-110">Nginx 背後的高效能的 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="045e5-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="045e5-111">Kestrel 都支援所有平台和.NET Core 支援的版本。</span><span class="sxs-lookup"><span data-stu-id="045e5-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="045e5-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="045e5-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="045e5-113">[檢視或下載範例程式碼 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="045e5-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="045e5-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="045e5-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="045e5-115">[檢視或下載範例程式碼 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="045e5-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="045e5-116">使用 Kestrel 透過反向 proxy 的時機</span><span class="sxs-lookup"><span data-stu-id="045e5-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="045e5-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="045e5-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="045e5-118">您可以單獨使用 Kestrel，或與 IIS、Nginx 或 Apache 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="045e5-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="045e5-119">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="045e5-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="045e5-122">如果 Kestrel 只公開到內部網路，則不論組態是否使用反向 Proxy 伺服器，皆可使用。</span><span class="sxs-lookup"><span data-stu-id="045e5-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="045e5-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="045e5-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="045e5-124">如果應用程式只接受來自內部網路的要求，您可以單獨使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="045e5-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="045e5-126">如果將應用程式公開到網際網路，您必須使用 IIS、Nginx 或 Apache 作為「反向 Proxy 伺服器」。</span><span class="sxs-lookup"><span data-stu-id="045e5-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="045e5-127">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="045e5-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="045e5-129">需要邊緣部署 （公開流量從網際網路） 的安全性考量的反向 proxy。</span><span class="sxs-lookup"><span data-stu-id="045e5-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="045e5-130">Kestrel 的 1.x 版對於攻擊的防禦並不充足。</span><span class="sxs-lookup"><span data-stu-id="045e5-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="045e5-131">其中包括但不限制於適當的逾時、 大小上限，以及並行連線限制。</span><span class="sxs-lookup"><span data-stu-id="045e5-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="045e5-132">您有多個共用相同的 IP 和連接埠將單一伺服器上執行的應用程式時需要的反向 proxy 的案例。</span><span class="sxs-lookup"><span data-stu-id="045e5-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="045e5-133">不會直接使用 Kestrel 因為 Kestrel 不支援共用相同的 IP 和多個處理序之間的連接埠。</span><span class="sxs-lookup"><span data-stu-id="045e5-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="045e5-134">當您設定 Kestrel 通訊埠上接聽時，它會處理該連接埠，無論主機標頭的所有流量。</span><span class="sxs-lookup"><span data-stu-id="045e5-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="045e5-135">反向 proxy 可以共用連接埠，然後必須轉送給 Kestrel 上唯一的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="045e5-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="045e5-136">即使反向 proxy 伺服器不是必要的使用其中一種可能很不錯的選擇對於其他原因：</span><span class="sxs-lookup"><span data-stu-id="045e5-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="045e5-137">它會限制公開的介面區。</span><span class="sxs-lookup"><span data-stu-id="045e5-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="045e5-138">它提供選擇性額外的組態並提供深層防禦。</span><span class="sxs-lookup"><span data-stu-id="045e5-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="045e5-139">它可能與現有基礎結構更好的整合。</span><span class="sxs-lookup"><span data-stu-id="045e5-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="045e5-140">它可簡化負載平衡和 SSL 設定。</span><span class="sxs-lookup"><span data-stu-id="045e5-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="045e5-141">只有您的反向 proxy 伺服器需要 SSL 憑證，且該伺服器與通訊使用一般 HTTP 內部網路上的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="045e5-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="045e5-142">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="045e5-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="045e5-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="045e5-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="045e5-144">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)封裝包含在[Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)。</span><span class="sxs-lookup"><span data-stu-id="045e5-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="045e5-145">依預設，ASP.NET Core 專案範本使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="045e5-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="045e5-146">在*Program.cs*，範本程式碼會呼叫`CreateDefaultBuilder`，而它會呼叫[UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)背後的運作原理。</span><span class="sxs-lookup"><span data-stu-id="045e5-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="045e5-147">如果您要設定 Kestrel 選項，請呼叫`UseKestrel`中*Program.cs*如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="045e5-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="045e5-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="045e5-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="045e5-149">安裝[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="045e5-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="045e5-150">呼叫[UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)上的擴充方法`WebHostBuilder`中您`Main`方法，並指定任何[Kestrel 選項](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)需要下一節中所示。</span><span class="sxs-lookup"><span data-stu-id="045e5-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="045e5-151">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="045e5-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="045e5-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="045e5-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="045e5-153">Kestrel web 伺服器的條件約束組態選項，在網際網路對向部署時特別有用。</span><span class="sxs-lookup"><span data-stu-id="045e5-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="045e5-154">以下是一些您可以設定的限制：</span><span class="sxs-lookup"><span data-stu-id="045e5-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="045e5-155">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="045e5-155">Maximum client connections</span></span>
- <span data-ttu-id="045e5-156">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="045e5-156">Maximum request body size</span></span>
- <span data-ttu-id="045e5-157">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="045e5-157">Minimum request body data rate</span></span>

<span data-ttu-id="045e5-158">您設定這些條件約束與其他人在`Limits`屬性[KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)類別。</span><span class="sxs-lookup"><span data-stu-id="045e5-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="045e5-159">`Limits`屬性會保留的執行個體[KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)類別。</span><span class="sxs-lookup"><span data-stu-id="045e5-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="045e5-160">**最大用戶端連線**</span><span class="sxs-lookup"><span data-stu-id="045e5-160">**Maximum client connections**</span></span>

<span data-ttu-id="045e5-161">同時開啟的 TCP 連線的數目上限可以設定整個應用程式，以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="045e5-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="045e5-162">沒有個別的限制已經從 HTTP 或 HTTPS 升級到另一個通訊協定 （例如，在 Websocket 要求） 的連線。</span><span class="sxs-lookup"><span data-stu-id="045e5-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="045e5-163">升級連線之後，它不會納入`MaxConcurrentConnections`限制。</span><span class="sxs-lookup"><span data-stu-id="045e5-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="045e5-164">最大連接數目是無限制 (null) 的預設值。</span><span class="sxs-lookup"><span data-stu-id="045e5-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="045e5-165">**要求主體大小上限**</span><span class="sxs-lookup"><span data-stu-id="045e5-165">**Maximum request body size**</span></span>

<span data-ttu-id="045e5-166">預設的最大要求本文大小是 30,000,000 位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="045e5-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="045e5-167">若要覆寫中的 ASP.NET Core MVC 應用程式的限制的建議的方式是使用[RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)動作方法上的屬性：</span><span class="sxs-lookup"><span data-stu-id="045e5-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="045e5-168">以下是範例，示範如何設定整個應用程式的每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="045e5-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="045e5-169">您可以覆寫特定中介軟體中的要求上的設定：</span><span class="sxs-lookup"><span data-stu-id="045e5-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="045e5-170">如果您嘗試將應用程式已開始讀取要求之後，在要求上設定的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="045e5-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="045e5-171">沒有`IsReadOnly`屬性會告訴您，如果`MaxRequestBodySize`屬性處於唯讀狀態，這表示它已經太遲設定的限制。</span><span class="sxs-lookup"><span data-stu-id="045e5-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="045e5-172">**最小要求主體資料的速率**</span><span class="sxs-lookup"><span data-stu-id="045e5-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="045e5-173">如果資料是中指定的速率位元組/秒 kestrel 會檢查每秒。</span><span class="sxs-lookup"><span data-stu-id="045e5-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="045e5-174">如果速率低於最小值、 連接逾時。在寬限期是時間的 Kestrel 可讓用戶端來增加其傳送速率的最小值量在這段期間，不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="045e5-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate is not checked during that time.</span></span> <span data-ttu-id="045e5-175">在寬限期內可協助避免中斷連接一開始是以低速由於 TCP 緩慢開始傳送資料。</span><span class="sxs-lookup"><span data-stu-id="045e5-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="045e5-176">預設最小頻率為 240 位元組/秒、 5 秒的寬限期內。</span><span class="sxs-lookup"><span data-stu-id="045e5-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="045e5-177">回應也適用於最小的速率。</span><span class="sxs-lookup"><span data-stu-id="045e5-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="045e5-178">程式碼以設定 要求限制和回應限制為相同，但是具有`RequestBody`或`Response`中的屬性和介面名稱。</span><span class="sxs-lookup"><span data-stu-id="045e5-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="045e5-179">以下是範例，示範如何設定中的最小的資料速率*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="045e5-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="045e5-180">您可以設定每個要求的速率中介軟體中,：</span><span class="sxs-lookup"><span data-stu-id="045e5-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="045e5-181">如需其他 Kestrel 選項的資訊，請參閱下列類別：</span><span class="sxs-lookup"><span data-stu-id="045e5-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="045e5-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="045e5-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="045e5-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="045e5-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="045e5-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="045e5-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="045e5-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="045e5-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="045e5-186">Kestrel 選項的相關資訊，請參閱[KestrelServerOptions 類別](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)。</span><span class="sxs-lookup"><span data-stu-id="045e5-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="045e5-187">端點組態</span><span class="sxs-lookup"><span data-stu-id="045e5-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="045e5-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="045e5-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="045e5-189">根據預設 ASP.NET Core 繫結至`http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="045e5-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="045e5-190">您設定 URL 前置詞和連接埠上接聽，藉由呼叫 Kestrel`Listen`或`ListenUnixSocket`方法`KestrelServerOptions`。</span><span class="sxs-lookup"><span data-stu-id="045e5-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="045e5-191">(`UseUrls`、`urls`命令列引數，並同樣 ASPNETCORE_URLS 環境變數，但沒有註明的限制[本文稍後](#useurls-limitations)。)</span><span class="sxs-lookup"><span data-stu-id="045e5-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="045e5-192">**繫結至 TCP 通訊端**</span><span class="sxs-lookup"><span data-stu-id="045e5-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="045e5-193">`Listen`方法繫結至 TCP 通訊端，而且選項 lambda，可讓您設定的 SSL 憑證：</span><span class="sxs-lookup"><span data-stu-id="045e5-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="045e5-194">請注意如何這個範例會設定 SSL 特定端點使用[ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)。</span><span class="sxs-lookup"><span data-stu-id="045e5-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="045e5-195">若要設定其他 Kestrel 設定之特定端點設定，您可以使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="045e5-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="045e5-196">**繫結至 Unix 通訊端**</span><span class="sxs-lookup"><span data-stu-id="045e5-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="045e5-197">您可以接聽 Unix 通訊端上以改善效能與 Nginx，本範例所示：</span><span class="sxs-lookup"><span data-stu-id="045e5-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="045e5-198">**連接埠 0**</span><span class="sxs-lookup"><span data-stu-id="045e5-198">**Port 0**</span></span>

<span data-ttu-id="045e5-199">如果您指定連接埠號碼 0，Kestrel 動態繫結至可用的通訊埠。</span><span class="sxs-lookup"><span data-stu-id="045e5-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="045e5-200">下列範例會示範如何判斷哪些實際上是 Kestrel 在執行階段繫結的連接埠：</span><span class="sxs-lookup"><span data-stu-id="045e5-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="045e5-201">**UseUrls 限制**</span><span class="sxs-lookup"><span data-stu-id="045e5-201">**UseUrls limitations**</span></span>

<span data-ttu-id="045e5-202">您可以設定端點，藉由呼叫`UseUrls`方法或使用`urls`命令列引數或 ASPNETCORE_URLS 環境變數。</span><span class="sxs-lookup"><span data-stu-id="045e5-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="045e5-203">這些方法會很有用，如果您希望程式碼以使用 Kestrel 以外的伺服器。</span><span class="sxs-lookup"><span data-stu-id="045e5-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="045e5-204">不過，請注意這些限制：</span><span class="sxs-lookup"><span data-stu-id="045e5-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="045e5-205">您無法使用 SSL，使用這些方法。</span><span class="sxs-lookup"><span data-stu-id="045e5-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="045e5-206">如果您同時使用`Listen`方法和`UseUrls`、`Listen`端點覆寫`UseUrls`端點。</span><span class="sxs-lookup"><span data-stu-id="045e5-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="045e5-207">**適用於 IIS 的端點組態**</span><span class="sxs-lookup"><span data-stu-id="045e5-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="045e5-208">如果您使用 IIS 時，IIS 的 URL 繫結覆寫任何您藉由呼叫所設定的繫結`Listen`或`UseUrls`。</span><span class="sxs-lookup"><span data-stu-id="045e5-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="045e5-209">如需詳細資訊，請參閱[簡介 ASP.NET 核心模組](aspnet-core-module.md)。</span><span class="sxs-lookup"><span data-stu-id="045e5-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="045e5-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="045e5-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="045e5-211">根據預設 ASP.NET Core 繫結至`http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="045e5-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="045e5-212">您可以設定 URL 前置詞和 Kestrel 接聽所使用的連接埠`UseUrls`擴充方法，`urls`命令列引數或 ASP.NET Core 組態系統。</span><span class="sxs-lookup"><span data-stu-id="045e5-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="045e5-213">如需有關這些方法的詳細資訊，請參閱[主控](../../fundamentals/hosting.md)。</span><span class="sxs-lookup"><span data-stu-id="045e5-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="045e5-214">當您使用 IIS 的反向 proxy，URL 繫結如何運作的詳細資訊，請參閱[ASP.NET 核心模組](aspnet-core-module.md)。</span><span class="sxs-lookup"><span data-stu-id="045e5-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="045e5-215">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="045e5-215">URL prefixes</span></span>

<span data-ttu-id="045e5-216">如果您呼叫`UseUrls`或使用`urls`命令列引數或 ASPNETCORE_URLS 環境變數，URL 前置詞可以是下列任一形式。</span><span class="sxs-lookup"><span data-stu-id="045e5-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="045e5-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="045e5-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="045e5-218">只有 HTTP URL 的前置詞是有效的;當您設定 URL 繫結使用 kestrel 不支援 SSL `UseUrls`。</span><span class="sxs-lookup"><span data-stu-id="045e5-218">Only HTTP URL prefixes are valid; Kestrel does not support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="045e5-219">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="045e5-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="045e5-220">0.0.0.0 是一個特殊情況，將繫結至所有 IPv4 位址。</span><span class="sxs-lookup"><span data-stu-id="045e5-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="045e5-221">IPv6 位址的連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="045e5-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="045e5-222">[::] 就 IPv6 相當於 IPv4 0.0.0.0。</span><span class="sxs-lookup"><span data-stu-id="045e5-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="045e5-223">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="045e5-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="045e5-224">主機名稱，\*，和 +，不是特殊。</span><span class="sxs-lookup"><span data-stu-id="045e5-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="045e5-225">任何不是可辨識的 IP 位址或"localhost"的項目會繫結的所有 IPv4 和 IPv6 Ip。</span><span class="sxs-lookup"><span data-stu-id="045e5-225">Anything that is not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="045e5-226">如果您要將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式相同的連接埠，請使用[HTTP.sys](httpsys.md)或如 IIS、 Nginx 或 Apache 反向 proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="045e5-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="045e5-227">"Localhost"的連接埠號碼的連接埠號碼或回送 IP 名稱</span><span class="sxs-lookup"><span data-stu-id="045e5-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="045e5-228">當`localhost`指定，則 Kestrel 嘗試繫結到 IPv4 和 IPv6 的回送介面。</span><span class="sxs-lookup"><span data-stu-id="045e5-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="045e5-229">如果要求的通訊埠，在任一個回送介面上的另一個服務使用 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="045e5-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="045e5-230">如果任一個回送介面，就無法使用任何其他原因 （大部分通常是因為不支援 IPv6 的時候），Kestrel 記錄警告。</span><span class="sxs-lookup"><span data-stu-id="045e5-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="045e5-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="045e5-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="045e5-232">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="045e5-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="045e5-233">0.0.0.0 是一個特殊情況，將繫結至所有 IPv4 位址。</span><span class="sxs-lookup"><span data-stu-id="045e5-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="045e5-234">IPv6 位址的連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="045e5-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="045e5-235">[::] 就 IPv6 相當於 IPv4 0.0.0.0。</span><span class="sxs-lookup"><span data-stu-id="045e5-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="045e5-236">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="045e5-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="045e5-237">主機名稱， \*，並不是特殊 +。</span><span class="sxs-lookup"><span data-stu-id="045e5-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="045e5-238">任何不是可辨識的 IP 位址或"localhost"的項目繫結至所有 IPv4 和 IPv6 Ip。</span><span class="sxs-lookup"><span data-stu-id="045e5-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="045e5-239">如果您要將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式相同的連接埠，請使用[WebListener](weblistener.md)或如 IIS、 Nginx 或 Apache 反向 proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="045e5-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="045e5-240">"Localhost"的連接埠號碼的連接埠號碼或回送 IP 名稱</span><span class="sxs-lookup"><span data-stu-id="045e5-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="045e5-241">當`localhost`指定，則 Kestrel 嘗試繫結到 IPv4 和 IPv6 的回送介面。</span><span class="sxs-lookup"><span data-stu-id="045e5-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="045e5-242">如果要求的通訊埠，在任一個回送介面上的另一個服務使用 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="045e5-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="045e5-243">如果任一個回送介面，就無法使用任何其他原因 （大部分通常是因為不支援 IPv6 的時候），Kestrel 記錄警告。</span><span class="sxs-lookup"><span data-stu-id="045e5-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="045e5-244">Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="045e5-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="045e5-245">**連接埠 0**</span><span class="sxs-lookup"><span data-stu-id="045e5-245">**Port 0**</span></span>

<span data-ttu-id="045e5-246">如果您指定連接埠號碼 0，Kestrel 動態繫結至可用的通訊埠。</span><span class="sxs-lookup"><span data-stu-id="045e5-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="045e5-247">繫結至連接埠 0 允許任何主機名稱或 IP 除了`localhost`名稱。</span><span class="sxs-lookup"><span data-stu-id="045e5-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="045e5-248">下列範例會示範如何判斷哪些實際上是 Kestrel 在執行階段繫結的連接埠：</span><span class="sxs-lookup"><span data-stu-id="045e5-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="045e5-249">**SSL 的 URL 前置詞**</span><span class="sxs-lookup"><span data-stu-id="045e5-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="045e5-250">務必包含 URL 前置詞與`https:`如果您呼叫`UseHttps`擴充方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="045e5-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> <span data-ttu-id="045e5-251">無法在相同的連接埠上裝載 HTTPS 和 HTTP。</span><span class="sxs-lookup"><span data-stu-id="045e5-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="045e5-252">後續步驟</span><span class="sxs-lookup"><span data-stu-id="045e5-252">Next steps</span></span>

<span data-ttu-id="045e5-253">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="045e5-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="045e5-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="045e5-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="045e5-255">2.x 的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="045e5-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="045e5-256">Kestrel 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="045e5-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="045e5-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="045e5-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="045e5-258">1.x 的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="045e5-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="045e5-259">Kestrel 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="045e5-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
