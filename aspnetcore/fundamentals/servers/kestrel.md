---
title: "ASP.NET Core 中的 Kestrel 網頁伺服器實作"
author: tdykstra
description: "介紹 Kestrel，這是以 libuv 為基礎的跨平台 ASP.NET Core 網頁伺服器。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="441e6-103">ASP.NET Core 中的 Kestrel 網頁伺服器實作簡介</span><span class="sxs-lookup"><span data-stu-id="441e6-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="441e6-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="441e6-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="441e6-105">Kestrel 是以 [libuv](https://github.com/libuv/libuv) (跨平台非同步 I/O 程式庫) 為基礎的跨平台 [ASP.NET Core 網路伺服器](index.md)。</span><span class="sxs-lookup"><span data-stu-id="441e6-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="441e6-106">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="441e6-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="441e6-107">Kestrel 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="441e6-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="441e6-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="441e6-108">HTTPS</span></span>
  * <span data-ttu-id="441e6-109">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="441e6-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="441e6-110">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="441e6-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="441e6-111">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="441e6-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="441e6-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="441e6-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="441e6-113">[檢視或下載 2.x 的範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="441e6-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="441e6-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="441e6-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="441e6-115">[檢視或下載 1.x 的範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="441e6-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="441e6-116">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="441e6-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="441e6-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="441e6-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="441e6-118">您可以單獨使用 Kestrel，或與 IIS、Nginx 或 Apache 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="441e6-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="441e6-119">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="441e6-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="441e6-122">如果 Kestrel 只公開到內部網路，則不論組態是否使用反向 Proxy 伺服器，皆可使用。</span><span class="sxs-lookup"><span data-stu-id="441e6-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="441e6-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="441e6-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="441e6-124">如果應用程式只接受來自內部網路的要求，您可以單獨使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="441e6-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="441e6-126">如果將應用程式公開到網際網路，您必須使用 IIS、Nginx 或 Apache 作為「反向 Proxy 伺服器」。</span><span class="sxs-lookup"><span data-stu-id="441e6-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="441e6-127">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="441e6-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="441e6-129">基於安全性考量，必須有反向 Proxy 才能進行邊緣部署 (公開到網際網路中的流量)。</span><span class="sxs-lookup"><span data-stu-id="441e6-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="441e6-130">Kestrel 的 1.x 版對於攻擊的防禦並不充足。</span><span class="sxs-lookup"><span data-stu-id="441e6-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="441e6-131">這包括但不限於適當的逾時、大小限制和同時連線限制。</span><span class="sxs-lookup"><span data-stu-id="441e6-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="441e6-132">需要反向 Proxy 的情況如下：您有多個在單一伺服器上執行的應用程式共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="441e6-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="441e6-133">不能直接使用 Kestrel，因為 Kestrel 不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="441e6-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="441e6-134">當您設定 Kestrel 接聽連接埠時，它會處理該連接埠的所有流量，而不管主機標頭為何。</span><span class="sxs-lookup"><span data-stu-id="441e6-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="441e6-135">可以共用連接埠的反向 Proxy 則必須在唯一的 IP 和連接埠上轉送給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="441e6-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="441e6-136">即使不需要反向 Proxy 伺服器，基於其他原因使用該伺服器也是不錯的選擇：</span><span class="sxs-lookup"><span data-stu-id="441e6-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="441e6-137">它可以限制公開的介面區。</span><span class="sxs-lookup"><span data-stu-id="441e6-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="441e6-138">它提供選擇性的額外組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="441e6-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="441e6-139">它能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="441e6-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="441e6-140">它簡化了負載平衡和 SSL 設定。</span><span class="sxs-lookup"><span data-stu-id="441e6-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="441e6-141">只有您的反向 Proxy 伺服器需要 SSL 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="441e6-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="441e6-142">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="441e6-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="441e6-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="441e6-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="441e6-144">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) 套件包含在 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)裡。</span><span class="sxs-lookup"><span data-stu-id="441e6-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="441e6-145">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="441e6-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="441e6-146">在 *Program.cs* 中，範本程式碼會呼叫 `CreateDefaultBuilder`，而後者會呼叫場景背後的 [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)。</span><span class="sxs-lookup"><span data-stu-id="441e6-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="441e6-147">如果您需要設定 Kestrel 選項，請在 *Program.cs* 中呼叫 `UseKestrel`，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="441e6-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="441e6-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="441e6-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="441e6-149">安裝 [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="441e6-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="441e6-150">在您的 `Main` 方法中，於 `WebHostBuilder` 上呼叫 [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 擴充方法，並指定需要的任何 [Kestrel 選項](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)，如下一節所示。</span><span class="sxs-lookup"><span data-stu-id="441e6-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="441e6-151">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="441e6-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="441e6-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="441e6-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="441e6-153">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="441e6-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="441e6-154">以下是一些您可以設定的限制：</span><span class="sxs-lookup"><span data-stu-id="441e6-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="441e6-155">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="441e6-155">Maximum client connections</span></span>
- <span data-ttu-id="441e6-156">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="441e6-156">Maximum request body size</span></span>
- <span data-ttu-id="441e6-157">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="441e6-157">Minimum request body data rate</span></span>

<span data-ttu-id="441e6-158">您可以在 [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) 類別的 `Limits` 屬性中設定這些條件約束和其他限制。</span><span class="sxs-lookup"><span data-stu-id="441e6-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="441e6-159">`Limits` 屬性會保留 [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="441e6-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="441e6-160">**用戶端連線數目上限**</span><span class="sxs-lookup"><span data-stu-id="441e6-160">**Maximum client connections**</span></span>

<span data-ttu-id="441e6-161">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="441e6-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="441e6-162">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="441e6-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="441e6-163">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="441e6-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="441e6-164">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="441e6-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="441e6-165">**要求主體大小上限**</span><span class="sxs-lookup"><span data-stu-id="441e6-165">**Maximum request body size**</span></span>

<span data-ttu-id="441e6-166">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6MB。</span><span class="sxs-lookup"><span data-stu-id="441e6-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="441e6-167">要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) 屬性：</span><span class="sxs-lookup"><span data-stu-id="441e6-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="441e6-168">以下範例會示範如何設定整個應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="441e6-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="441e6-169">您可以在中介軟體中覆寫特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="441e6-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="441e6-170">如果您嘗試在應用程式已開始讀取要求之後，設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="441e6-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="441e6-171">有一個 `IsReadOnly` 屬性會告訴您 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="441e6-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="441e6-172">**要求主體資料速率下限**</span><span class="sxs-lookup"><span data-stu-id="441e6-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="441e6-173">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="441e6-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="441e6-174">如果速率低於下限值，則連線會逾時。寬限期是 Kestrel 提供給用戶端的時間量，以便將其傳送速率提高到下限值；在這段期間不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="441e6-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="441e6-175">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="441e6-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="441e6-176">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="441e6-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="441e6-177">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="441e6-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="441e6-178">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="441e6-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="441e6-179">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="441e6-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="441e6-180">您可以在中介軟體中設定每個要求的速率：</span><span class="sxs-lookup"><span data-stu-id="441e6-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="441e6-181">如需其他 Kestrel 選項的資訊，請參閱下列類別：</span><span class="sxs-lookup"><span data-stu-id="441e6-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="441e6-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="441e6-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="441e6-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="441e6-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="441e6-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="441e6-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="441e6-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="441e6-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="441e6-186">如需 Kestrel 選項的資訊，請參閱 [KestrelServerOptions 類別](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)。</span><span class="sxs-lookup"><span data-stu-id="441e6-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="441e6-187">端點組態</span><span class="sxs-lookup"><span data-stu-id="441e6-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="441e6-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="441e6-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="441e6-189">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="441e6-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="441e6-190">藉由在 `KestrelServerOptions` 上 呼叫 `Listen` 或 `ListenUnixSocket` 方法，您可以設定 URL 前置詞和讓 Kestrel 接聽的連接埠。</span><span class="sxs-lookup"><span data-stu-id="441e6-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="441e6-191">(`UseUrls`、`urls` 命令列引數和 ASPNETCORE_URLS 環境變數同樣有效，但卻有[本文稍後](#useurls-limitations)註明的限制。)</span><span class="sxs-lookup"><span data-stu-id="441e6-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="441e6-192">**繫結至 TCP 通訊端**</span><span class="sxs-lookup"><span data-stu-id="441e6-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="441e6-193">`Listen` 方法繫結至 TCP 通訊端，而選項 Lambda 可讓您設定 SSL 憑證：</span><span class="sxs-lookup"><span data-stu-id="441e6-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="441e6-194">請注意這個範例如何使用 [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs) 設定特定端點的 SSL。</span><span class="sxs-lookup"><span data-stu-id="441e6-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="441e6-195">若要設定特定端點的其他 Kestrel 設定，您可以使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="441e6-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="441e6-196">**繫結至 Unix 通訊端**</span><span class="sxs-lookup"><span data-stu-id="441e6-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="441e6-197">您可以接聽 Unix 通訊端以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="441e6-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="441e6-198">**連接埠 0**</span><span class="sxs-lookup"><span data-stu-id="441e6-198">**Port 0**</span></span>

<span data-ttu-id="441e6-199">如果您指定連接埠號碼 0，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="441e6-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="441e6-200">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="441e6-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="441e6-201">**UseUrls 限制**</span><span class="sxs-lookup"><span data-stu-id="441e6-201">**UseUrls limitations**</span></span>

<span data-ttu-id="441e6-202">藉由呼叫 `UseUrls` 方法，或者使用 `urls` 命令列引數或 ASPNETCORE_URLS 環境變數，您可以設定端點。</span><span class="sxs-lookup"><span data-stu-id="441e6-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="441e6-203">如果您希望程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="441e6-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="441e6-204">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="441e6-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="441e6-205">您無法使用 SSL 與這些方法搭配。</span><span class="sxs-lookup"><span data-stu-id="441e6-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="441e6-206">如果您同時使用 `Listen` 方法和 `UseUrls`，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="441e6-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="441e6-207">**IIS 的端點組態**</span><span class="sxs-lookup"><span data-stu-id="441e6-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="441e6-208">如果您使用 IIS，IIS 的 URL 繫結就會覆寫您呼叫 `Listen` 或 `UseUrls` 所設定的任何繫結。</span><span class="sxs-lookup"><span data-stu-id="441e6-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="441e6-209">如需詳細資訊，請參閱 [ASP.NET Core 模組簡介](aspnet-core-module.md)。</span><span class="sxs-lookup"><span data-stu-id="441e6-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="441e6-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="441e6-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="441e6-211">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="441e6-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="441e6-212">藉由使用 `UseUrls` 擴充方法、`urls` 命令列引數或 ASP.NET Core 組態系統，您可以設定 URL 前置詞和 Kestrel 所要接聽的連接埠。</span><span class="sxs-lookup"><span data-stu-id="441e6-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="441e6-213">如需這些方法的詳細資訊，請參閱[裝載](../../fundamentals/hosting.md)。</span><span class="sxs-lookup"><span data-stu-id="441e6-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="441e6-214">如需當您使用 IIS 作為反向 Proxy 時，URL 繫結如何運作的資訊，請參閱 [ASP.NET Core 模組](aspnet-core-module.md)。</span><span class="sxs-lookup"><span data-stu-id="441e6-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="441e6-215">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="441e6-215">URL prefixes</span></span>

<span data-ttu-id="441e6-216">如果您呼叫 `UseUrls`，或是使用 `urls` 命令列引數或 ASPNETCORE_URLS 環境變數，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="441e6-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="441e6-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="441e6-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="441e6-218">只有 HTTP 的 URL 前置詞才有效；當您使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 SSL 。</span><span class="sxs-lookup"><span data-stu-id="441e6-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="441e6-219">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="441e6-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="441e6-220">0.0.0.0 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="441e6-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="441e6-221">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="441e6-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="441e6-222">[::] 是相當於 IPv4 0.0.0.0 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="441e6-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="441e6-223">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="441e6-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="441e6-224">主機名稱 \* 和 + 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="441e6-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="441e6-225">不是可辨識的 IP 位址或 "localhost" 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="441e6-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="441e6-226">如果您需要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](httpsys.md) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="441e6-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="441e6-227">"Localhost" 與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="441e6-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="441e6-228">如果指定了 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="441e6-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="441e6-229">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="441e6-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="441e6-230">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="441e6-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="441e6-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="441e6-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="441e6-232">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="441e6-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="441e6-233">0.0.0.0 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="441e6-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="441e6-234">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="441e6-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="441e6-235">[::] 是相當於 IPv4 0.0.0.0 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="441e6-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="441e6-236">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="441e6-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="441e6-237">主機名稱 \* 和 + 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="441e6-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="441e6-238">不是可辨識的 IP 位址或 "localhost" 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="441e6-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="441e6-239">如果您需要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [WebListener](weblistener.md) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="441e6-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="441e6-240">"Localhost" 與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="441e6-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="441e6-241">如果指定了 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="441e6-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="441e6-242">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="441e6-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="441e6-243">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="441e6-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="441e6-244">UNIX 通訊端</span><span class="sxs-lookup"><span data-stu-id="441e6-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="441e6-245">**連接埠 0**</span><span class="sxs-lookup"><span data-stu-id="441e6-245">**Port 0**</span></span>

<span data-ttu-id="441e6-246">如果您指定連接埠號碼 0，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="441e6-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="441e6-247">除了 `localhost` 名稱，任何主機名稱或 IP 都允許繫結至連接埠 0。</span><span class="sxs-lookup"><span data-stu-id="441e6-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="441e6-248">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="441e6-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="441e6-249">**SSL 的 URL 前置詞**</span><span class="sxs-lookup"><span data-stu-id="441e6-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="441e6-250">如果您呼叫 `UseHttps` 擴充方法，請務必使用 `https:` 包含 URL 前置詞，如下所示。</span><span class="sxs-lookup"><span data-stu-id="441e6-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="441e6-251">HTTPS 和 HTTP 無法裝載在相同的連接埠上。</span><span class="sxs-lookup"><span data-stu-id="441e6-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="441e6-252">後續步驟</span><span class="sxs-lookup"><span data-stu-id="441e6-252">Next steps</span></span>

<span data-ttu-id="441e6-253">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="441e6-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="441e6-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="441e6-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="441e6-255">2.x 的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="441e6-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="441e6-256">Kestrel 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="441e6-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="441e6-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="441e6-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="441e6-258">1.x 的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="441e6-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="441e6-259">Kestrel 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="441e6-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
