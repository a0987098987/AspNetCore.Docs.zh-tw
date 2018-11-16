---
title: ASP.NET Core 中的 Kestrel 網頁伺服器實作
author: guardrex
description: 了解 Kestrel，這是 ASP.NET Core 的跨平台網頁伺服器。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/05/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: a26726a824db31e07b881dbfa8dc2ef37d4d3492
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505761"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="8efea-103">ASP.NET Core 中的 Kestrel 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="8efea-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="8efea-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="8efea-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="8efea-105">如需本主題的 1.1 版，請下載 [ASP.NET Core (1.1 版，PDF) 中的 Kestrel 網頁伺服器實作](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf)。</span><span class="sxs-lookup"><span data-stu-id="8efea-105">For the 1.1 version of this topic, download [Kestrel web server implementation in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf).</span></span>

<span data-ttu-id="8efea-106">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="8efea-106">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="8efea-107">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="8efea-107">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="8efea-108">Kestrel 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="8efea-108">Kestrel supports the following features:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="8efea-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="8efea-109">HTTPS</span></span>
* <span data-ttu-id="8efea-110">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="8efea-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="8efea-111">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="8efea-111">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="8efea-112">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="8efea-112">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="8efea-113">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="8efea-113">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="8efea-114">HTTPS</span><span class="sxs-lookup"><span data-stu-id="8efea-114">HTTPS</span></span>
* <span data-ttu-id="8efea-115">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="8efea-115">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="8efea-116">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="8efea-116">Unix sockets for high performance behind Nginx</span></span>

::: moniker-end

<span data-ttu-id="8efea-117">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="8efea-117">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="8efea-118">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8efea-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a><span data-ttu-id="8efea-119">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="8efea-119">HTTP/2 support</span></span>

<span data-ttu-id="8efea-120">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="8efea-120">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="8efea-121">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="8efea-121">Operating system&dagger;</span></span>
  * <span data-ttu-id="8efea-122">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="8efea-122">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="8efea-123">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="8efea-123">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="8efea-124">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="8efea-124">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="8efea-125">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="8efea-125">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="8efea-126">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="8efea-126">TLS 1.2 or later connection</span></span>

<span data-ttu-id="8efea-127">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="8efea-127">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="8efea-128">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="8efea-128">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="8efea-129">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="8efea-129">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="8efea-130">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="8efea-130">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="8efea-131">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="8efea-131">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="8efea-132">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="8efea-132">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="8efea-133">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="8efea-133">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="8efea-134">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="8efea-134">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="8efea-135">您可以單獨使用 Kestrel，或與 IIS、Nginx 或 Apache 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="8efea-135">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="8efea-136">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="8efea-136">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="8efea-139">不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式，其中之一都是有效且支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="8efea-139">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

<span data-ttu-id="8efea-140">反向 Proxy 存在的情況如下：有多個在單一伺服器上執行的應用程式共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="8efea-140">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="8efea-141">Kestrel 不支援這種情況，因為 Kestrel 不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="8efea-141">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="8efea-142">當 Kestrel 設定為接聽通訊埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的主機標頭。</span><span class="sxs-lookup"><span data-stu-id="8efea-142">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="8efea-143">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="8efea-143">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="8efea-144">即使不需要反向 Proxy 伺服器，反向 Proxy 伺服器也是不錯的選擇：</span><span class="sxs-lookup"><span data-stu-id="8efea-144">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="8efea-145">它可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="8efea-145">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="8efea-146">它提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="8efea-146">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="8efea-147">它能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="8efea-147">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="8efea-148">它簡化負載平衡和 SSL 組態。</span><span class="sxs-lookup"><span data-stu-id="8efea-148">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="8efea-149">只有反向 Proxy 伺服器需要 SSL 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="8efea-149">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="8efea-150">如果未使用反向 Proxy 並啟用主機篩選，則必須啟用[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="8efea-150">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="8efea-151">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="8efea-151">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="8efea-152">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) 套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中 (ASP.NET Core 2.1 或更新版本)。</span><span class="sxs-lookup"><span data-stu-id="8efea-152">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="8efea-153">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="8efea-153">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="8efea-154">在 *Program.cs* 中，範本程式碼會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)，而後者會呼叫場景背後的 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel)。</span><span class="sxs-lookup"><span data-stu-id="8efea-154">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8efea-155">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請使用 `ConfigureKestrel`：</span><span class="sxs-lookup"><span data-stu-id="8efea-155">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8efea-156">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請呼叫 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel)：</span><span class="sxs-lookup"><span data-stu-id="8efea-156">To provide additional configuration after calling `CreateDefaultBuilder`, call [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

## <a name="kestrel-options"></a><span data-ttu-id="8efea-157">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="8efea-157">Kestrel options</span></span>

<span data-ttu-id="8efea-158">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="8efea-158">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="8efea-159">請在 [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) 類別的 [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) 屬性中設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="8efea-159">Set constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="8efea-160">`Limits` 屬性會保留 [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="8efea-160">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

### <a name="maximum-client-connections"></a><span data-ttu-id="8efea-161">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="8efea-161">Maximum client connections</span></span>

[<span data-ttu-id="8efea-162">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="8efea-162">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="8efea-163">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="8efea-163">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="8efea-164">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="8efea-164">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="8efea-165">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="8efea-165">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="8efea-166">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="8efea-166">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="8efea-167">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="8efea-167">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="8efea-168">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="8efea-168">Maximum request body size</span></span>

[<span data-ttu-id="8efea-169">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="8efea-169">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="8efea-170">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="8efea-170">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="8efea-171">要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 屬性：</span><span class="sxs-lookup"><span data-stu-id="8efea-171">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="8efea-172">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="8efea-172">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="8efea-173">您可以在中介軟體中覆寫特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="8efea-173">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

<span data-ttu-id="8efea-174">如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="8efea-174">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="8efea-175">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="8efea-175">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="8efea-176">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="8efea-176">Minimum request body data rate</span></span>

[<span data-ttu-id="8efea-177">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="8efea-177">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="8efea-178">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="8efea-178">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="8efea-179">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="8efea-179">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="8efea-180">如果速率低於下限值，則連線會逾時。寬限期是 Kestrel 提供給用戶端的時間量，以便將其傳送速率提高到下限值；在這段期間不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="8efea-180">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="8efea-181">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="8efea-181">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="8efea-182">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="8efea-182">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="8efea-183">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="8efea-183">A minimum rate also applies to the response.</span></span> <span data-ttu-id="8efea-184">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="8efea-184">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="8efea-185">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="8efea-185">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

::: moniker-end

<span data-ttu-id="8efea-186">您可以覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="8efea-186">You can override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8efea-187">因為通訊協定對要求多工的支援，所以 HTTP/2 不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的所有速率功能都不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="8efea-187">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="8efea-188">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="8efea-188">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="8efea-189">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="8efea-189">Maximum streams per connection</span></span>

<span data-ttu-id="8efea-190">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="8efea-190">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="8efea-191">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="8efea-191">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="8efea-192">預設值為 100。</span><span class="sxs-lookup"><span data-stu-id="8efea-192">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="8efea-193">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="8efea-193">Header table size</span></span>

<span data-ttu-id="8efea-194">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="8efea-194">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="8efea-195">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="8efea-195">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="8efea-196">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="8efea-196">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="8efea-197">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="8efea-197">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="8efea-198">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="8efea-198">Maximum frame size</span></span>

<span data-ttu-id="8efea-199">`Http2.MaxFrameSize` 表示要接收的 HTTP/2 連線框架承載大小上限。</span><span class="sxs-lookup"><span data-stu-id="8efea-199">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="8efea-200">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="8efea-200">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="8efea-201">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="8efea-201">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="8efea-202">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="8efea-202">Maximum request header size</span></span>

<span data-ttu-id="8efea-203">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="8efea-203">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="8efea-204">此限制皆共同套用至已壓縮及未壓縮代表中的名稱與值。</span><span class="sxs-lookup"><span data-stu-id="8efea-204">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="8efea-205">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="8efea-205">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="8efea-206">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="8efea-206">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="8efea-207">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="8efea-207">Initial connection window size</span></span>

<span data-ttu-id="8efea-208">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="8efea-208">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="8efea-209">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="8efea-209">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="8efea-210">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="8efea-210">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="8efea-211">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="8efea-211">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="8efea-212">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="8efea-212">Initial stream window size</span></span>

<span data-ttu-id="8efea-213">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="8efea-213">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="8efea-214">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="8efea-214">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="8efea-215">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="8efea-215">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="8efea-216">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="8efea-216">The default value is 96 KB (98,304).</span></span>

::: moniker-end

<span data-ttu-id="8efea-217">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="8efea-217">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="8efea-218">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="8efea-218">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="8efea-219">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="8efea-219">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="8efea-220">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="8efea-220">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

## <a name="endpoint-configuration"></a><span data-ttu-id="8efea-221">端點組態</span><span class="sxs-lookup"><span data-stu-id="8efea-221">Endpoint configuration</span></span>

<span data-ttu-id="8efea-222">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="8efea-222">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="8efea-223">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="8efea-223">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="8efea-224">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="8efea-224">A development certificate is created:</span></span>

* <span data-ttu-id="8efea-225">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="8efea-225">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="8efea-226">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-226">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="8efea-227">有些瀏覽器需要您授與明確的權限給瀏覽器，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-227">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="8efea-228">ASP.NET Core 2.1 和更新版本的專案範本會將應用程式設定為預設於 HTTPS 上執行，並包含 [HTTPS 重新導向和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="8efea-228">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="8efea-229">在 [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) 上呼叫 [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) 或 [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) 方法，以設定 Kestrel 的 URL 前置詞和連接埠。</span><span class="sxs-lookup"><span data-stu-id="8efea-229">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="8efea-230">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="8efea-230">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="8efea-231">ASP.NET Core 2.1 `KestrelServerOptions` 組態：</span><span class="sxs-lookup"><span data-stu-id="8efea-231">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a><span data-ttu-id="8efea-232">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="8efea-232">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span></span>

<span data-ttu-id="8efea-233">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="8efea-233">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="8efea-234">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="8efea-234">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a><span data-ttu-id="8efea-235">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="8efea-235">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span></span>

<span data-ttu-id="8efea-236">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="8efea-236">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="8efea-237">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="8efea-237">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="8efea-238">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="8efea-238">Configure(IConfiguration)</span></span>

<span data-ttu-id="8efea-239">建立組態載入器，設定採用 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="8efea-239">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="8efea-240">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="8efea-240">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="8efea-241">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="8efea-241">ListenOptions.UseHttps</span></span>

<span data-ttu-id="8efea-242">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="8efea-242">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="8efea-243">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="8efea-243">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="8efea-244">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="8efea-244">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="8efea-245">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="8efea-245">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="8efea-246">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="8efea-246">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="8efea-247">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="8efea-247">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="8efea-248">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="8efea-248">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="8efea-249">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="8efea-249">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="8efea-250">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="8efea-250">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="8efea-251">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="8efea-251">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="8efea-252">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="8efea-252">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="8efea-253">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-253">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="8efea-254">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="8efea-254">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="8efea-255">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-255">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="8efea-256">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="8efea-256">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="8efea-257">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-257">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="8efea-258">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="8efea-258">Supported configurations described next:</span></span>

* <span data-ttu-id="8efea-259">無組態</span><span class="sxs-lookup"><span data-stu-id="8efea-259">No configuration</span></span>
* <span data-ttu-id="8efea-260">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="8efea-260">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="8efea-261">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="8efea-261">Change the defaults in code</span></span>

<span data-ttu-id="8efea-262">*無組態*</span><span class="sxs-lookup"><span data-stu-id="8efea-262">*No configuration*</span></span>

<span data-ttu-id="8efea-263">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="8efea-263">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="8efea-264">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="8efea-264">Specify URLs using the:</span></span>

* <span data-ttu-id="8efea-265">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="8efea-265">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="8efea-266">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="8efea-266">`--urls` command-line argument.</span></span>
* <span data-ttu-id="8efea-267">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8efea-267">`urls` host configuration key.</span></span>
* <span data-ttu-id="8efea-268">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="8efea-268">`UseUrls` extension method.</span></span>

<span data-ttu-id="8efea-269">如需詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="8efea-269">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="8efea-270">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="8efea-270">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="8efea-271">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000;http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="8efea-271">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="8efea-272">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="8efea-272">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="8efea-273">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 組態。</span><span class="sxs-lookup"><span data-stu-id="8efea-273">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="8efea-274">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="8efea-274">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="8efea-275">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="8efea-275">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="8efea-276">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="8efea-276">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="8efea-277">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="8efea-277">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="8efea-278">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證]  > [預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-278">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="8efea-279">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-279">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="8efea-280">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="8efea-280">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="8efea-281">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="8efea-281">Schema notes:</span></span>

* <span data-ttu-id="8efea-282">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="8efea-282">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="8efea-283">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="8efea-283">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="8efea-284">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="8efea-284">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="8efea-285">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="8efea-285">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="8efea-286">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="8efea-286">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="8efea-287">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="8efea-287">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="8efea-288">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="8efea-288">The `Certificate` section is optional.</span></span> <span data-ttu-id="8efea-289">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="8efea-289">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="8efea-290">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="8efea-290">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="8efea-291">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-291">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="8efea-292">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="8efea-292">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="8efea-293">`options.Configure(context.Configuration.GetSection("Kestrel"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, options => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="8efea-293">`options.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="8efea-294">您也可以直接存取 `KestrelServerOptions.ConfigurationLoader` 來保持反覆運算現有的載入器，例如 [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 提供的載入器。</span><span class="sxs-lookup"><span data-stu-id="8efea-294">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="8efea-295">每個端點的組態區段可用於 `Endpoint` 方法的選項，因此可讀取自訂組態。</span><span class="sxs-lookup"><span data-stu-id="8efea-295">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="8efea-296">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("Kestrel"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="8efea-296">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="8efea-297">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="8efea-297">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="8efea-298">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="8efea-298">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="8efea-299">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="8efea-299">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="8efea-300">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="8efea-300">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="8efea-301">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="8efea-301">*Change the defaults in code*</span></span>

<span data-ttu-id="8efea-302">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-302">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="8efea-303">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="8efea-303">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

<span data-ttu-id="8efea-304">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="8efea-304">*Kestrel support for SNI*</span></span>

<span data-ttu-id="8efea-305">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="8efea-305">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="8efea-306">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-306">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="8efea-307">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="8efea-307">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="8efea-308">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="8efea-308">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="8efea-309">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="8efea-309">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="8efea-310">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="8efea-310">SNI support requires:</span></span>

* <span data-ttu-id="8efea-311">在目標 Framework `netcoreapp2.1` 上執行。</span><span class="sxs-lookup"><span data-stu-id="8efea-311">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="8efea-312">在 `netcoreapp2.0` 和 `net461` 上，會叫用回呼，但 `name` 一律為 `null`。</span><span class="sxs-lookup"><span data-stu-id="8efea-312">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="8efea-313">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="8efea-313">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="8efea-314">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="8efea-314">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="8efea-315">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="8efea-315">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="8efea-316">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="8efea-316">Bind to a TCP socket</span></span>

<span data-ttu-id="8efea-317">[Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) 方法繫結至 TCP 通訊端，而選項 Lambda 則允許 SSL 憑證組態：</span><span class="sxs-lookup"><span data-stu-id="8efea-317">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

<span data-ttu-id="8efea-318">範例使用 [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions) 來設定端點的 SSL。</span><span class="sxs-lookup"><span data-stu-id="8efea-318">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="8efea-319">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="8efea-319">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="8efea-320">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="8efea-320">Bind to a Unix socket</span></span>

<span data-ttu-id="8efea-321">使用 [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) 接聽 UNIX 通訊端以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="8efea-321">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

### <a name="port-0"></a><span data-ttu-id="8efea-322">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="8efea-322">Port 0</span></span>

<span data-ttu-id="8efea-323">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="8efea-323">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="8efea-324">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="8efea-324">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="8efea-325">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="8efea-325">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="8efea-326">限制</span><span class="sxs-lookup"><span data-stu-id="8efea-326">Limitations</span></span>

<span data-ttu-id="8efea-327">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="8efea-327">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="8efea-328">UseUrls</span><span class="sxs-lookup"><span data-stu-id="8efea-328">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="8efea-329">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="8efea-329">`--urls` command-line argument</span></span>
* <span data-ttu-id="8efea-330">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="8efea-330">`urls` host configuration key</span></span>
* <span data-ttu-id="8efea-331">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="8efea-331">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="8efea-332">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="8efea-332">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="8efea-333">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="8efea-333">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="8efea-334">SSL 無法搭配這些方法使用，除非在 HTTPS 端點組態中提供預設憑證 (例如，使用 `KestrelServerOptions` 組態或組態檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="8efea-334">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="8efea-335">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="8efea-335">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="8efea-336">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="8efea-336">IIS endpoint configuration</span></span>

<span data-ttu-id="8efea-337">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="8efea-337">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="8efea-338">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="8efea-338">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a><span data-ttu-id="8efea-339">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="8efea-339">ListenOptions.Protocols</span></span>

<span data-ttu-id="8efea-340">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="8efea-340">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="8efea-341">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="8efea-341">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="8efea-342">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="8efea-342">`HttpProtocols` enum value</span></span> | <span data-ttu-id="8efea-343">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="8efea-343">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="8efea-344">僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="8efea-344">HTTP/1.1 only.</span></span> <span data-ttu-id="8efea-345">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="8efea-345">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="8efea-346">僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="8efea-346">HTTP/2 only.</span></span> <span data-ttu-id="8efea-347">主要在具有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="8efea-347">Primarily used with TLS.</span></span> <span data-ttu-id="8efea-348">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="8efea-348">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="8efea-349">HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="8efea-349">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="8efea-350">需要 TLS 和 [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線才能交涉 HTTP/2；否則，連線預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="8efea-350">Requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection to negotiate HTTP/2; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="8efea-351">預設通訊協定為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="8efea-351">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="8efea-352">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="8efea-352">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="8efea-353">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="8efea-353">TLS version 1.2 or later</span></span>
* <span data-ttu-id="8efea-354">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="8efea-354">Renegotiation disabled</span></span>
* <span data-ttu-id="8efea-355">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="8efea-355">Compression disabled</span></span>
* <span data-ttu-id="8efea-356">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="8efea-356">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="8efea-357">橢圓曲線 Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 最小 224 個位元</span><span class="sxs-lookup"><span data-stu-id="8efea-357">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="8efea-358">有限欄位 Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 最小 2048 個位元</span><span class="sxs-lookup"><span data-stu-id="8efea-358">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="8efea-359">加密套件未列於封鎖清單中</span><span class="sxs-lookup"><span data-stu-id="8efea-359">Cipher suite not blacklisted</span></span>

<span data-ttu-id="8efea-360">預設支援 `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; 與 P-256 橢圓曲線 &lbrack;`FIPS186`&rbrack;。</span><span class="sxs-lookup"><span data-stu-id="8efea-360">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="8efea-361">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="8efea-361">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="8efea-362">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="8efea-362">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="8efea-363">選擇性地建立 `IConnectionAdapter` 實作，針對每個連線來篩選特定加密的 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="8efea-363">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
                listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
            });
        });
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that you don't 
        // wish to support. Alternatively, define and compare 
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher 
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null 
        // indicates that no cipher algorithm supported by Kestrel matches the 
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

<span data-ttu-id="8efea-364">從設定進行通訊協定設定</span><span class="sxs-lookup"><span data-stu-id="8efea-364">*Set the protocol from configuration*</span></span>

<span data-ttu-id="8efea-365">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 組態。</span><span class="sxs-lookup"><span data-stu-id="8efea-365">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="8efea-366">在下列 *appsettings.json* 範例中，會為 Kestrel 的所有端點建立預設連線通訊協定 (HTTP/1.1 和 HTTP/2)：</span><span class="sxs-lookup"><span data-stu-id="8efea-366">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="8efea-367">下列設定檔範例會為特定端點建立連線通訊協定：</span><span class="sxs-lookup"><span data-stu-id="8efea-367">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="8efea-368">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="8efea-368">Protocols specified in code override values set by configuration.</span></span>

::: moniker-end

## <a name="transport-configuration"></a><span data-ttu-id="8efea-369">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="8efea-369">Transport configuration</span></span>

<span data-ttu-id="8efea-370">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="8efea-370">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="8efea-371">對於升級到 2.1 的 ASP.NET Core 2.0 應用程式而言，這是一項重大變更，它們會呼叫 [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) 並相依於下列任一套件：</span><span class="sxs-lookup"><span data-stu-id="8efea-371">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="8efea-372">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="8efea-372">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="8efea-373">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="8efea-373">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="8efea-374">對於使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)且需要使用 Libuv 的 ASP.NET Core 2.1 或更新版本專案：</span><span class="sxs-lookup"><span data-stu-id="8efea-374">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="8efea-375">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="8efea-375">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* <span data-ttu-id="8efea-376">呼叫 [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv)：</span><span class="sxs-lookup"><span data-stu-id="8efea-376">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

### <a name="url-prefixes"></a><span data-ttu-id="8efea-377">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="8efea-377">URL prefixes</span></span>

<span data-ttu-id="8efea-378">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="8efea-378">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="8efea-379">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="8efea-379">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="8efea-380">當使用 `UseUrls` 設定 URL 繫結時，Kestrel 不支援 SSL。</span><span class="sxs-lookup"><span data-stu-id="8efea-380">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="8efea-381">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="8efea-381">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="8efea-382">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="8efea-382">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="8efea-383">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="8efea-383">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="8efea-384">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="8efea-384">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="8efea-385">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="8efea-385">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="8efea-386">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="8efea-386">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="8efea-387">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="8efea-387">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="8efea-388">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="8efea-388">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="8efea-389">如果未使用反向 Proxy 並啟用主機篩選，請啟用[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="8efea-389">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="8efea-390">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="8efea-390">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="8efea-391">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="8efea-391">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="8efea-392">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="8efea-392">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="8efea-393">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="8efea-393">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="8efea-394">主機篩選</span><span class="sxs-lookup"><span data-stu-id="8efea-394">Host filtering</span></span>

<span data-ttu-id="8efea-395">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="8efea-395">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="8efea-396">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="8efea-396">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="8efea-397">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="8efea-397">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="8efea-398">此資訊完全不用來驗證要求 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="8efea-398">None of this information is used to validate request `Host` headers.</span></span>

<span data-ttu-id="8efea-399">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="8efea-399">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="8efea-400">主機篩選中介軟體係由 [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) 套件提供，隨附於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本)。</span><span class="sxs-lookup"><span data-stu-id="8efea-400">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="8efea-401">中介軟體是由 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 新增，它會呼叫 [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering)：</span><span class="sxs-lookup"><span data-stu-id="8efea-401">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="8efea-402">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="8efea-402">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="8efea-403">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8efea-403">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="8efea-404">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="8efea-404">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="8efea-405">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="8efea-405">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="8efea-406">[轉送標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) 選項。</span><span class="sxs-lookup"><span data-stu-id="8efea-406">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="8efea-407">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="8efea-407">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="8efea-408">當不保留主機標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="8efea-408">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="8efea-409">當使用 Kestrel 作為公眾面向邊緣伺服器，或直接轉送主機標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="8efea-409">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="8efea-410">如需轉送標頭中介軟體的詳細資訊，請參閱[設定 ASP.NET Core 以與 Proxy 伺服器和負載平衡器搭配運作](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="8efea-410">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8efea-411">其他資源</span><span class="sxs-lookup"><span data-stu-id="8efea-411">Additional resources</span></span>

* [<span data-ttu-id="8efea-412">強制使用 HTTPS</span><span class="sxs-lookup"><span data-stu-id="8efea-412">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="8efea-413">Kestrel 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="8efea-413">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="8efea-414">RFC 7230：訊息語法和路由 (第 5.4 節：主機)</span><span class="sxs-lookup"><span data-stu-id="8efea-414">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
* [<span data-ttu-id="8efea-415">設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器</span><span class="sxs-lookup"><span data-stu-id="8efea-415">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
