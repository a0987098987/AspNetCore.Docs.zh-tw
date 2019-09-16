---
title: ASP.NET Core 中的 Kestrel 網頁伺服器實作
author: guardrex
description: 了解 Kestrel，這是 ASP.NET Core 的跨平台網頁伺服器。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: b1c28f084e67d9cce74258433aa0c884878c2520
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011168"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="38255-103">ASP.NET Core 中的 Kestrel 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="38255-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="38255-104">由[Tom 作者: dykstra](https://github.com/tdykstra)、 [Chris Ross](https://github.com/Tratcher)、 [Stephen Halter](https://twitter.com/halter73)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="38255-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="38255-105">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="38255-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="38255-106">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="38255-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="38255-107">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="38255-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="38255-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="38255-108">HTTPS</span></span>
* <span data-ttu-id="38255-109">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="38255-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="38255-110">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="38255-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="38255-111">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="38255-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="38255-112">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="38255-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="38255-113">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="38255-114">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="38255-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="38255-115">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="38255-115">HTTP/2 support</span></span>

<span data-ttu-id="38255-116">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="38255-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="38255-117">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="38255-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="38255-118">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="38255-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="38255-119">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="38255-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="38255-120">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="38255-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="38255-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="38255-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="38255-122">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="38255-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="38255-123">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="38255-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="38255-124">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="38255-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="38255-125">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="38255-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="38255-126">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="38255-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="38255-127">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="38255-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="38255-128">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="38255-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="38255-129">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="38255-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="38255-130">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="38255-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="38255-131">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="38255-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="38255-132">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="38255-133">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="38255-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="38255-135">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="38255-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="38255-137">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="38255-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="38255-138">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="38255-139">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="38255-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="38255-140">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="38255-141">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="38255-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="38255-142">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="38255-142">A reverse proxy:</span></span>

* <span data-ttu-id="38255-143">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="38255-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="38255-144">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="38255-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="38255-145">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="38255-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="38255-146">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="38255-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="38255-147">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="38255-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="38255-148">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="38255-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="38255-149">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="38255-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="38255-150">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="38255-151">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="38255-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="38255-152">若要在呼叫`CreateDefaultBuilder`和`ConfigureWebHostDefaults`之後提供額外的`ConfigureKestrel`設定，請使用：</span><span class="sxs-lookup"><span data-stu-id="38255-152">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseStartup<Startup>();
        });
```

<span data-ttu-id="38255-153">如果應用程式未呼叫 `CreateDefaultBuilder` 來設定主機，請在呼叫 `ConfigureKestrel` **之前**，先呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="38255-153">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new HostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseIISIntegration()
            .UseStartup<Startup>();
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="38255-154">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="38255-154">Kestrel options</span></span>

<span data-ttu-id="38255-155">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="38255-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="38255-156">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="38255-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="38255-157">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="38255-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="38255-158">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="38255-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="38255-159">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="38255-159">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="38255-160">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="38255-160">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="38255-161">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="38255-161">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="38255-162">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="38255-162">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="38255-163">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="38255-163">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="38255-164">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="38255-164">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="38255-165">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="38255-165">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="38255-166">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="38255-166">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="38255-167">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="38255-167">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="38255-168">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="38255-168">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="38255-169">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="38255-169">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="38255-170">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="38255-170">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="38255-171">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="38255-171">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="38255-172">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="38255-172">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="38255-173">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="38255-173">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="38255-174">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="38255-174">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="38255-175">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="38255-175">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="38255-176">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="38255-176">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="38255-177">如果速率低於下限值，則連線會逾時。寬限期是 Kestrel 提供給用戶端的時間量，以便將其傳送速率提高到下限值；在這段期間不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="38255-177">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="38255-178">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="38255-178">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="38255-179">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="38255-179">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="38255-180">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="38255-180">A minimum rate also applies to the response.</span></span> <span data-ttu-id="38255-181">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="38255-181">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="38255-182">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="38255-182">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="38255-183">覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="38255-183">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="38255-184">因為通訊協定對要求多工的支援，所以 HTTP/2 一般不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> 不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="38255-184">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="38255-185">不過，<xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> 仍存在 HTTP/2 要求的 `HttpContext.Features`您仍能透過將 `IHttpMinRequestBodyDataRateFeature.MinDataRate` 設定為 `null` (即使是針對 HTTP/2 要求)，以個別要求基礎來「完全停用」讀取素率限制。</span><span class="sxs-lookup"><span data-stu-id="38255-185">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="38255-186">嘗試讀取 `IHttpMinRequestBodyDataRateFeature.MinDataRate` 或嘗試將它設定為 `null` 以外的值將會導致擲回 `NotSupportedException` (假設要求是 HTTP/2 要求)。</span><span class="sxs-lookup"><span data-stu-id="38255-186">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="38255-187">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="38255-187">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="38255-188">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="38255-188">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="38255-189">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="38255-189">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="38255-190">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="38255-190">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="38255-191">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="38255-191">Maximum streams per connection</span></span>

<span data-ttu-id="38255-192">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="38255-192">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="38255-193">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="38255-193">Excess streams are refused.</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="38255-194">預設值是 100。</span><span class="sxs-lookup"><span data-stu-id="38255-194">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="38255-195">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="38255-195">Header table size</span></span>

<span data-ttu-id="38255-196">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="38255-196">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="38255-197">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="38255-197">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="38255-198">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="38255-198">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="38255-199">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="38255-199">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="38255-200">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="38255-200">Maximum frame size</span></span>

<span data-ttu-id="38255-201">`Http2.MaxFrameSize`指出伺服器所接收或傳送之 HTTP/2 連接框架承載的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="38255-201">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="38255-202">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="38255-202">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="38255-203">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="38255-203">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="38255-204">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="38255-204">Maximum request header size</span></span>

<span data-ttu-id="38255-205">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="38255-205">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="38255-206">這項限制適用于其壓縮和未壓縮標記法中的名稱和值。</span><span class="sxs-lookup"><span data-stu-id="38255-206">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="38255-207">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="38255-207">The value must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="38255-208">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="38255-208">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="38255-209">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="38255-209">Initial connection window size</span></span>

<span data-ttu-id="38255-210">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="38255-210">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="38255-211">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="38255-211">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="38255-212">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="38255-212">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="38255-213">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="38255-213">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="38255-214">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="38255-214">Initial stream window size</span></span>

<span data-ttu-id="38255-215">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="38255-215">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="38255-216">要求也皆受 `Http2.InitialConnectionWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="38255-216">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="38255-217">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="38255-217">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="38255-218">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="38255-218">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="38255-219">同步 IO</span><span class="sxs-lookup"><span data-stu-id="38255-219">Synchronous IO</span></span>

<span data-ttu-id="38255-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> 控制是否允許要求與回應的同步 IO。</span><span class="sxs-lookup"><span data-stu-id="38255-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="38255-221">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="38255-221">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="38255-222">大量的封鎖同步 IO 作業會導致執行緒集區耗盡，這會使得應用程式沒有回應。</span><span class="sxs-lookup"><span data-stu-id="38255-222">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="38255-223">只有當使用不支援同步 IO 的程式庫時才啟用 `AllowSynchronousIO`。</span><span class="sxs-lookup"><span data-stu-id="38255-223">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="38255-224">下列範例會啟用同步 IO：</span><span class="sxs-lookup"><span data-stu-id="38255-224">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="38255-225">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="38255-225">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="38255-226">端點組態</span><span class="sxs-lookup"><span data-stu-id="38255-226">Endpoint configuration</span></span>

<span data-ttu-id="38255-227">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="38255-227">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="38255-228">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="38255-228">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="38255-229">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="38255-229">Specify URLs using the:</span></span>

* <span data-ttu-id="38255-230">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="38255-230">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="38255-231">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="38255-231">`--urls` command-line argument.</span></span>
* <span data-ttu-id="38255-232">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="38255-232">`urls` host configuration key.</span></span>
* <span data-ttu-id="38255-233">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="38255-233">`UseUrls` extension method.</span></span>

<span data-ttu-id="38255-234">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="38255-234">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="38255-235">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000; http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="38255-235">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="38255-236">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="38255-236">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="38255-237">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="38255-237">A development certificate is created:</span></span>

* <span data-ttu-id="38255-238">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="38255-238">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="38255-239">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-239">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="38255-240">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-240">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="38255-241">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="38255-241">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="38255-242">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-242">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="38255-243">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="38255-243">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="38255-244">`KestrelServerOptions`配置</span><span class="sxs-lookup"><span data-stu-id="38255-244">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="38255-245">ConfigureEndpointDefaults （Action\<listenoptions 來 >）</span><span class="sxs-lookup"><span data-stu-id="38255-245">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="38255-246">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="38255-246">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="38255-247">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="38255-247">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="38255-248">ConfigureHttpsDefaults （Action\<HttpsConnectionAdapterOptions >）</span><span class="sxs-lookup"><span data-stu-id="38255-248">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="38255-249">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="38255-249">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="38255-250">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="38255-250">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    // certificate is an X509Certificate2
                    listenOptions.ServerCertificate = certificate;
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configureiconfiguration"></a><span data-ttu-id="38255-251">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="38255-251">Configure(IConfiguration)</span></span>

<span data-ttu-id="38255-252">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-252">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="38255-253">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="38255-253">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="38255-254">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="38255-254">ListenOptions.UseHttps</span></span>

<span data-ttu-id="38255-255">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-255">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="38255-256">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="38255-256">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="38255-257">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="38255-257">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="38255-258">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="38255-258">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="38255-259">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="38255-259">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="38255-260">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="38255-260">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="38255-261">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="38255-261">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="38255-262">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="38255-262">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="38255-263">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="38255-263">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="38255-264">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="38255-264">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="38255-265">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="38255-265">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="38255-266">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-266">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="38255-267">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="38255-267">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="38255-268">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-268">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="38255-269">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-269">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="38255-270">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-270">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="38255-271">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="38255-271">Supported configurations described next:</span></span>

* <span data-ttu-id="38255-272">無組態</span><span class="sxs-lookup"><span data-stu-id="38255-272">No configuration</span></span>
* <span data-ttu-id="38255-273">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="38255-273">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="38255-274">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="38255-274">Change the defaults in code</span></span>

<span data-ttu-id="38255-275">*無組態*</span><span class="sxs-lookup"><span data-stu-id="38255-275">*No configuration*</span></span>

<span data-ttu-id="38255-276">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="38255-276">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="38255-277">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="38255-277">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="38255-278">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="38255-278">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="38255-279">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="38255-279">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="38255-280">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="38255-280">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="38255-281">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="38255-281">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="38255-282">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="38255-282">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="38255-283">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-283">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="38255-284">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-284">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="38255-285">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="38255-285">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="38255-286">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="38255-286">Schema notes:</span></span>

* <span data-ttu-id="38255-287">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="38255-287">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="38255-288">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="38255-288">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="38255-289">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="38255-289">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="38255-290">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="38255-290">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="38255-291">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="38255-291">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="38255-292">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="38255-292">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="38255-293">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="38255-293">The `Certificate` section is optional.</span></span> <span data-ttu-id="38255-294">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="38255-294">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="38255-295">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="38255-295">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="38255-296">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-296">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="38255-297">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="38255-297">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="38255-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="38255-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel((context, serverOptions) =>
            {
                serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                    .Endpoint("HTTPS", listenOptions =>
                    {
                        listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                    });
            });
        });
```

<span data-ttu-id="38255-299">`KestrelServerOptions.ConfigurationLoader`可以直接存取，以繼續逐一查看現有的載入器，例如所提供<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>的載入器。</span><span class="sxs-lookup"><span data-stu-id="38255-299">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="38255-300">每個端點的組態區段可用於 `Endpoint` 方法的選項，因此可讀取自訂組態。</span><span class="sxs-lookup"><span data-stu-id="38255-300">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="38255-301">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="38255-301">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="38255-302">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="38255-302">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="38255-303">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="38255-303">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="38255-304">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="38255-304">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="38255-305">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="38255-305">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="38255-306">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="38255-306">*Change the defaults in code*</span></span>

<span data-ttu-id="38255-307">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-307">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="38255-308">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="38255-308">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });

                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    listenOptions.SslProtocols = SslProtocols.Tls12;
                });
            });
        });
```

<span data-ttu-id="38255-309">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="38255-309">*Kestrel support for SNI*</span></span>

<span data-ttu-id="38255-310">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="38255-310">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="38255-311">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-311">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="38255-312">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="38255-312">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="38255-313">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="38255-313">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="38255-314">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-314">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="38255-315">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="38255-315">SNI support requires:</span></span>

* <span data-ttu-id="38255-316">在目標 framework `netcoreapp2.1`或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="38255-316">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="38255-317">在`net461`或更新版本上，會叫用回呼`name` ，但`null`一律為。</span><span class="sxs-lookup"><span data-stu-id="38255-317">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="38255-318">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="38255-318">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="38255-319">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="38255-319">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="38255-320">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-320">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ListenAnyIP(5005, listenOptions =>
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
            .UseStartup<Startup>();
        });
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="38255-321">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="38255-321">Bind to a TCP socket</span></span>

<span data-ttu-id="38255-322"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="38255-322">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="38255-323">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-323">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="38255-324">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="38255-324">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="38255-325">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="38255-325">Bind to a Unix socket</span></span>

<span data-ttu-id="38255-326">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="38255-326">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="38255-327">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="38255-327">Port 0</span></span>

<span data-ttu-id="38255-328">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-328">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="38255-329">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="38255-329">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="38255-330">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="38255-330">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="38255-331">限制</span><span class="sxs-lookup"><span data-stu-id="38255-331">Limitations</span></span>

<span data-ttu-id="38255-332">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="38255-332">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="38255-333">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="38255-333">`--urls` command-line argument</span></span>
* <span data-ttu-id="38255-334">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="38255-334">`urls` host configuration key</span></span>
* <span data-ttu-id="38255-335">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="38255-335">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="38255-336">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="38255-336">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="38255-337">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="38255-337">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="38255-338">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="38255-338">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="38255-339">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="38255-339">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="38255-340">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="38255-340">IIS endpoint configuration</span></span>

<span data-ttu-id="38255-341">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="38255-341">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="38255-342">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="38255-342">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="38255-343">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="38255-343">ListenOptions.Protocols</span></span>

<span data-ttu-id="38255-344">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="38255-344">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="38255-345">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="38255-345">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="38255-346">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="38255-346">`HttpProtocols` enum value</span></span> | <span data-ttu-id="38255-347">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="38255-347">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="38255-348">僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="38255-348">HTTP/1.1 only.</span></span> <span data-ttu-id="38255-349">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="38255-349">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="38255-350">僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="38255-350">HTTP/2 only.</span></span> <span data-ttu-id="38255-351">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="38255-351">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="38255-352">HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="38255-352">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="38255-353">HTTP/2 要求用戶端選取 TLS[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)交握中的 HTTP/2;否則，連接預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="38255-353">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="38255-354">任何端點`ListenOptions.Protocols`的預設值為。 `HttpProtocols.Http1AndHttp2`</span><span class="sxs-lookup"><span data-stu-id="38255-354">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="38255-355">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="38255-355">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="38255-356">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="38255-356">TLS version 1.2 or later</span></span>
* <span data-ttu-id="38255-357">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="38255-357">Renegotiation disabled</span></span>
* <span data-ttu-id="38255-358">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="38255-358">Compression disabled</span></span>
* <span data-ttu-id="38255-359">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="38255-359">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="38255-360">橢圓曲線 Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 最小 224 個位元</span><span class="sxs-lookup"><span data-stu-id="38255-360">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="38255-361">有限欄位 Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 最小 2048 個位元</span><span class="sxs-lookup"><span data-stu-id="38255-361">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="38255-362">加密套件未列於封鎖清單中</span><span class="sxs-lookup"><span data-stu-id="38255-362">Cipher suite not blacklisted</span></span>

<span data-ttu-id="38255-363">預設支援 `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; 與 P-256 橢圓曲線 &lbrack;`FIPS186`&rbrack;。</span><span class="sxs-lookup"><span data-stu-id="38255-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="38255-364">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="38255-364">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="38255-365">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="38255-365">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="38255-366">選擇性地使用連接中介軟體，針對特定的加密，針對每個連線篩選 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="38255-366">Optionally use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseConnectionHandler<TlsFilterConnectionHandler>();
    });
});
```

```csharp
using System;
using System.Buffers;
using System.Security.Authentication;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Connections;
using Microsoft.AspNetCore.Connections.Features;

public class TlsFilterConnectionHandler : ConnectionHandler
{
    public override async Task OnConnectedAsync(ConnectionContext connection)
    {
        var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
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

        while (true)
        {
            var result = await connection.Transport.Input.ReadAsync();
            var buffer = result.Buffer;

            if (!buffer.IsEmpty)
            {
                await connection.Transport.Output.WriteAsync(buffer.ToArray());
            }
            else if (result.IsCompleted)
            {
                break;
            }

            connection.Transport.Input.AdvanceTo(buffer.End);
        }
    }
}
```

<span data-ttu-id="38255-367">從設定進行通訊協定設定</span><span class="sxs-lookup"><span data-stu-id="38255-367">*Set the protocol from configuration*</span></span>

<span data-ttu-id="38255-368">`CreateDefaultBuilder` 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="38255-368">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="38255-369">下列*appsettings*範例會建立 HTTP/1.1 作為所有端點的預設連接通訊協定：</span><span class="sxs-lookup"><span data-stu-id="38255-369">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="38255-370">下列*appsettings*範例會建立特定端點的 HTTP/1.1 連接通訊協定：</span><span class="sxs-lookup"><span data-stu-id="38255-370">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1"
      }
    }
  }
}
```

<span data-ttu-id="38255-371">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="38255-371">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="38255-372">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="38255-372">Transport configuration</span></span>

<span data-ttu-id="38255-373">針對需要使用 Libuv （<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>）的專案：</span><span class="sxs-lookup"><span data-stu-id="38255-373">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="38255-374">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="38255-374">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="38255-375">在上呼叫<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>： `IWebHostBuilder`</span><span class="sxs-lookup"><span data-stu-id="38255-375">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateHostBuilder(args).Build().Run();
       }

       public static IHostBuilder CreateHostBuilder(string[] args) =>
           Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
               {
                   webBuilder.UseLibuv();
                   webBuilder.UseStartup<Startup>();
               });
   }
   ```

### <a name="url-prefixes"></a><span data-ttu-id="38255-376">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="38255-376">URL prefixes</span></span>

<span data-ttu-id="38255-377">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="38255-377">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="38255-378">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="38255-378">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="38255-379">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-379">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="38255-380">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-380">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="38255-381">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="38255-381">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="38255-382">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-382">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="38255-383">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="38255-383">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="38255-384">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-384">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="38255-385">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="38255-385">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="38255-386">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="38255-386">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="38255-387">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="38255-387">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="38255-388">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="38255-388">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="38255-389">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-389">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="38255-390">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="38255-390">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="38255-391">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="38255-391">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="38255-392">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="38255-392">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="38255-393">主機篩選</span><span class="sxs-lookup"><span data-stu-id="38255-393">Host filtering</span></span>

<span data-ttu-id="38255-394">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="38255-394">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="38255-395">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="38255-395">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="38255-396">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="38255-396">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="38255-397">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="38255-397">`Host` headers aren't validated.</span></span>

<span data-ttu-id="38255-398">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38255-398">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="38255-399">主機篩選中介軟體是由[AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件提供，這是針對 ASP.NET Core 應用程式而隱含提供的。</span><span class="sxs-lookup"><span data-stu-id="38255-399">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="38255-400">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="38255-400">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="38255-401">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38255-401">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="38255-402">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="38255-402">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="38255-403">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="38255-403">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="38255-404">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="38255-404">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="38255-405">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="38255-405">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="38255-406">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="38255-406">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="38255-407">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="38255-407">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="38255-408">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="38255-408">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="38255-409">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="38255-409">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="38255-410">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="38255-410">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="38255-411">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="38255-411">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="38255-412">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="38255-412">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="38255-413">HTTPS</span><span class="sxs-lookup"><span data-stu-id="38255-413">HTTPS</span></span>
* <span data-ttu-id="38255-414">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="38255-414">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="38255-415">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="38255-415">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="38255-416">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="38255-416">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="38255-417">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="38255-417">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="38255-418">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-418">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="38255-419">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="38255-419">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="38255-420">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="38255-420">HTTP/2 support</span></span>

<span data-ttu-id="38255-421">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="38255-421">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="38255-422">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="38255-422">Operating system&dagger;</span></span>
  * <span data-ttu-id="38255-423">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="38255-423">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="38255-424">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="38255-424">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="38255-425">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="38255-425">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="38255-426">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="38255-426">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="38255-427">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="38255-427">TLS 1.2 or later connection</span></span>

<span data-ttu-id="38255-428">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="38255-428">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="38255-429">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="38255-429">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="38255-430">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="38255-430">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="38255-431">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="38255-431">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="38255-432">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="38255-432">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="38255-433">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="38255-433">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="38255-434">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="38255-434">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="38255-435">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="38255-435">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="38255-436">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="38255-436">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="38255-437">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-437">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="38255-438">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="38255-438">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="38255-440">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="38255-440">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="38255-442">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="38255-442">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="38255-443">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-443">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="38255-444">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="38255-444">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="38255-445">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-445">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="38255-446">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="38255-446">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="38255-447">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="38255-447">A reverse proxy:</span></span>

* <span data-ttu-id="38255-448">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="38255-448">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="38255-449">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="38255-449">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="38255-450">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="38255-450">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="38255-451">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="38255-451">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="38255-452">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="38255-452">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="38255-453">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="38255-453">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="38255-454">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="38255-454">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="38255-455">[Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)套件包含在 AspNetCore 中。[應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="38255-455">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="38255-456">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-456">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="38255-457">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="38255-457">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="38255-458">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請使用 `ConfigureKestrel`：</span><span class="sxs-lookup"><span data-stu-id="38255-458">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="38255-459">如果應用程式未呼叫 `CreateDefaultBuilder` 來設定主機，請在呼叫 `ConfigureKestrel` **之前**，先呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="38255-459">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="38255-460">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="38255-460">Kestrel options</span></span>

<span data-ttu-id="38255-461">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="38255-461">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="38255-462">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="38255-462">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="38255-463">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="38255-463">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="38255-464">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="38255-464">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="38255-465">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="38255-465">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="38255-466">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="38255-466">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="38255-467">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="38255-467">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="38255-468">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="38255-468">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="38255-469">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="38255-469">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="38255-470">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="38255-470">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="38255-471">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="38255-471">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="38255-472">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="38255-472">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="38255-473">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="38255-473">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="38255-474">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="38255-474">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="38255-475">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="38255-475">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="38255-476">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="38255-476">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="38255-477">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="38255-477">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="38255-478">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="38255-478">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="38255-479">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="38255-479">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="38255-480">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="38255-480">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="38255-481">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="38255-481">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="38255-482">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="38255-482">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="38255-483">如果速率低於下限值，則連線會逾時。寬限期是 Kestrel 提供給用戶端的時間量，以便將其傳送速率提高到下限值；在這段期間不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="38255-483">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="38255-484">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="38255-484">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="38255-485">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="38255-485">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="38255-486">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="38255-486">A minimum rate also applies to the response.</span></span> <span data-ttu-id="38255-487">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="38255-487">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="38255-488">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="38255-488">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="38255-489">覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="38255-489">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="38255-490">因為通訊協定對要求多工的支援，所以 HTTP/2 不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的所有速率功能都不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="38255-490">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="38255-491">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="38255-491">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="38255-492">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="38255-492">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="38255-493">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="38255-493">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="38255-494">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="38255-494">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="38255-495">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="38255-495">Maximum streams per connection</span></span>

<span data-ttu-id="38255-496">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="38255-496">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="38255-497">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="38255-497">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="38255-498">預設值是 100。</span><span class="sxs-lookup"><span data-stu-id="38255-498">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="38255-499">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="38255-499">Header table size</span></span>

<span data-ttu-id="38255-500">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="38255-500">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="38255-501">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="38255-501">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="38255-502">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="38255-502">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="38255-503">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="38255-503">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="38255-504">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="38255-504">Maximum frame size</span></span>

<span data-ttu-id="38255-505">`Http2.MaxFrameSize` 表示要接收的 HTTP/2 連線框架承載大小上限。</span><span class="sxs-lookup"><span data-stu-id="38255-505">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="38255-506">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="38255-506">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="38255-507">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="38255-507">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="38255-508">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="38255-508">Maximum request header size</span></span>

<span data-ttu-id="38255-509">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="38255-509">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="38255-510">此限制皆共同套用至已壓縮及未壓縮代表中的名稱與值。</span><span class="sxs-lookup"><span data-stu-id="38255-510">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="38255-511">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="38255-511">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="38255-512">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="38255-512">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="38255-513">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="38255-513">Initial connection window size</span></span>

<span data-ttu-id="38255-514">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="38255-514">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="38255-515">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="38255-515">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="38255-516">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="38255-516">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="38255-517">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="38255-517">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="38255-518">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="38255-518">Initial stream window size</span></span>

<span data-ttu-id="38255-519">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="38255-519">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="38255-520">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="38255-520">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="38255-521">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="38255-521">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="38255-522">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="38255-522">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="38255-523">同步 IO</span><span class="sxs-lookup"><span data-stu-id="38255-523">Synchronous IO</span></span>

<span data-ttu-id="38255-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> 控制是否允許要求與回應的同步 IO。</span><span class="sxs-lookup"><span data-stu-id="38255-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="38255-525">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="38255-525">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="38255-526">大量的封鎖同步 IO 作業會導致執行緒集區耗盡，這會使得應用程式沒有回應。</span><span class="sxs-lookup"><span data-stu-id="38255-526">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="38255-527">只有當使用不支援同步 IO 的程式庫時才啟用 `AllowSynchronousIO`。</span><span class="sxs-lookup"><span data-stu-id="38255-527">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="38255-528">下列範例會啟用同步 IO：</span><span class="sxs-lookup"><span data-stu-id="38255-528">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="38255-529">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="38255-529">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="38255-530">端點組態</span><span class="sxs-lookup"><span data-stu-id="38255-530">Endpoint configuration</span></span>

<span data-ttu-id="38255-531">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="38255-531">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="38255-532">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="38255-532">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="38255-533">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="38255-533">Specify URLs using the:</span></span>

* <span data-ttu-id="38255-534">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="38255-534">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="38255-535">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="38255-535">`--urls` command-line argument.</span></span>
* <span data-ttu-id="38255-536">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="38255-536">`urls` host configuration key.</span></span>
* <span data-ttu-id="38255-537">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="38255-537">`UseUrls` extension method.</span></span>

<span data-ttu-id="38255-538">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="38255-538">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="38255-539">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000; http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="38255-539">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="38255-540">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="38255-540">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="38255-541">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="38255-541">A development certificate is created:</span></span>

* <span data-ttu-id="38255-542">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="38255-542">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="38255-543">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-543">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="38255-544">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-544">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="38255-545">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="38255-545">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="38255-546">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-546">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="38255-547">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="38255-547">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="38255-548">`KestrelServerOptions`配置</span><span class="sxs-lookup"><span data-stu-id="38255-548">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="38255-549">ConfigureEndpointDefaults （Action\<listenoptions 來 >）</span><span class="sxs-lookup"><span data-stu-id="38255-549">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="38255-550">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="38255-550">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="38255-551">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="38255-551">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="38255-552">ConfigureHttpsDefaults （Action\<HttpsConnectionAdapterOptions >）</span><span class="sxs-lookup"><span data-stu-id="38255-552">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="38255-553">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="38255-553">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="38255-554">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="38255-554">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="38255-555">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="38255-555">Configure(IConfiguration)</span></span>

<span data-ttu-id="38255-556">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-556">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="38255-557">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="38255-557">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="38255-558">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="38255-558">ListenOptions.UseHttps</span></span>

<span data-ttu-id="38255-559">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-559">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="38255-560">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="38255-560">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="38255-561">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="38255-561">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="38255-562">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="38255-562">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="38255-563">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="38255-563">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="38255-564">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="38255-564">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="38255-565">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="38255-565">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="38255-566">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="38255-566">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="38255-567">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="38255-567">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="38255-568">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="38255-568">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="38255-569">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="38255-569">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="38255-570">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-570">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="38255-571">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="38255-571">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="38255-572">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-572">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="38255-573">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-573">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="38255-574">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-574">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="38255-575">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="38255-575">Supported configurations described next:</span></span>

* <span data-ttu-id="38255-576">無組態</span><span class="sxs-lookup"><span data-stu-id="38255-576">No configuration</span></span>
* <span data-ttu-id="38255-577">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="38255-577">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="38255-578">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="38255-578">Change the defaults in code</span></span>

<span data-ttu-id="38255-579">*無組態*</span><span class="sxs-lookup"><span data-stu-id="38255-579">*No configuration*</span></span>

<span data-ttu-id="38255-580">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="38255-580">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="38255-581">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="38255-581">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="38255-582">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="38255-582">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="38255-583">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="38255-583">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="38255-584">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="38255-584">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="38255-585">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="38255-585">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="38255-586">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="38255-586">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="38255-587">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-587">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="38255-588">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-588">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="38255-589">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="38255-589">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="38255-590">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="38255-590">Schema notes:</span></span>

* <span data-ttu-id="38255-591">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="38255-591">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="38255-592">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="38255-592">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="38255-593">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="38255-593">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="38255-594">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="38255-594">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="38255-595">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="38255-595">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="38255-596">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="38255-596">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="38255-597">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="38255-597">The `Certificate` section is optional.</span></span> <span data-ttu-id="38255-598">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="38255-598">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="38255-599">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="38255-599">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="38255-600">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-600">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="38255-601">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="38255-601">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="38255-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="38255-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="38255-603">`KestrelServerOptions.ConfigurationLoader`可以直接存取，以繼續逐一查看現有的載入器，例如所提供<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>的載入器。</span><span class="sxs-lookup"><span data-stu-id="38255-603">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="38255-604">每個端點的組態區段可用於 `Endpoint` 方法的選項，因此可讀取自訂組態。</span><span class="sxs-lookup"><span data-stu-id="38255-604">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="38255-605">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="38255-605">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="38255-606">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="38255-606">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="38255-607">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="38255-607">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="38255-608">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="38255-608">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="38255-609">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="38255-609">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="38255-610">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="38255-610">*Change the defaults in code*</span></span>

<span data-ttu-id="38255-611">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-611">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="38255-612">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="38255-612">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="38255-613">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="38255-613">*Kestrel support for SNI*</span></span>

<span data-ttu-id="38255-614">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="38255-614">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="38255-615">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-615">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="38255-616">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="38255-616">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="38255-617">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="38255-617">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="38255-618">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-618">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="38255-619">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="38255-619">SNI support requires:</span></span>

* <span data-ttu-id="38255-620">在目標 framework `netcoreapp2.1`或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="38255-620">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="38255-621">在`net461`或更新版本上，會叫用回呼`name` ，但`null`一律為。</span><span class="sxs-lookup"><span data-stu-id="38255-621">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="38255-622">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="38255-622">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="38255-623">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="38255-623">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="38255-624">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-624">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="38255-625">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="38255-625">Bind to a TCP socket</span></span>

<span data-ttu-id="38255-626"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="38255-626">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="38255-627">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-627">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="38255-628">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="38255-628">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="38255-629">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="38255-629">Bind to a Unix socket</span></span>

<span data-ttu-id="38255-630">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="38255-630">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="38255-631">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="38255-631">Port 0</span></span>

<span data-ttu-id="38255-632">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-632">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="38255-633">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="38255-633">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="38255-634">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="38255-634">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="38255-635">限制</span><span class="sxs-lookup"><span data-stu-id="38255-635">Limitations</span></span>

<span data-ttu-id="38255-636">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="38255-636">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="38255-637">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="38255-637">`--urls` command-line argument</span></span>
* <span data-ttu-id="38255-638">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="38255-638">`urls` host configuration key</span></span>
* <span data-ttu-id="38255-639">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="38255-639">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="38255-640">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="38255-640">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="38255-641">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="38255-641">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="38255-642">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="38255-642">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="38255-643">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="38255-643">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="38255-644">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="38255-644">IIS endpoint configuration</span></span>

<span data-ttu-id="38255-645">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="38255-645">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="38255-646">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="38255-646">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="38255-647">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="38255-647">ListenOptions.Protocols</span></span>

<span data-ttu-id="38255-648">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="38255-648">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="38255-649">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="38255-649">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="38255-650">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="38255-650">`HttpProtocols` enum value</span></span> | <span data-ttu-id="38255-651">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="38255-651">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="38255-652">僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="38255-652">HTTP/1.1 only.</span></span> <span data-ttu-id="38255-653">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="38255-653">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="38255-654">僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="38255-654">HTTP/2 only.</span></span> <span data-ttu-id="38255-655">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="38255-655">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="38255-656">HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="38255-656">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="38255-657">HTTP/2 需要 TLS 和[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)連線;否則，連接預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="38255-657">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="38255-658">預設通訊協定為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="38255-658">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="38255-659">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="38255-659">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="38255-660">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="38255-660">TLS version 1.2 or later</span></span>
* <span data-ttu-id="38255-661">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="38255-661">Renegotiation disabled</span></span>
* <span data-ttu-id="38255-662">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="38255-662">Compression disabled</span></span>
* <span data-ttu-id="38255-663">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="38255-663">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="38255-664">橢圓曲線 Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 最小 224 個位元</span><span class="sxs-lookup"><span data-stu-id="38255-664">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="38255-665">有限欄位 Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 最小 2048 個位元</span><span class="sxs-lookup"><span data-stu-id="38255-665">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="38255-666">加密套件未列於封鎖清單中</span><span class="sxs-lookup"><span data-stu-id="38255-666">Cipher suite not blacklisted</span></span>

<span data-ttu-id="38255-667">預設支援 `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; 與 P-256 橢圓曲線 &lbrack;`FIPS186`&rbrack;。</span><span class="sxs-lookup"><span data-stu-id="38255-667">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="38255-668">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="38255-668">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="38255-669">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="38255-669">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="38255-670">選擇性地建立 `IConnectionAdapter` 實作，針對每個連線來篩選特定加密的 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="38255-670">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
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

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
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

<span data-ttu-id="38255-671">從設定進行通訊協定設定</span><span class="sxs-lookup"><span data-stu-id="38255-671">*Set the protocol from configuration*</span></span>

<span data-ttu-id="38255-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="38255-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="38255-673">在下列 *appsettings.json* 範例中，會為 Kestrel 的所有端點建立預設連線通訊協定 (HTTP/1.1 和 HTTP/2)：</span><span class="sxs-lookup"><span data-stu-id="38255-673">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="38255-674">下列設定檔範例會為特定端點建立連線通訊協定：</span><span class="sxs-lookup"><span data-stu-id="38255-674">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="38255-675">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="38255-675">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="38255-676">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="38255-676">Transport configuration</span></span>

<span data-ttu-id="38255-677">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="38255-677">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="38255-678">對於升級到 2.1 且會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> 並相依於下列任一套件的 ASP.NET Core 2.0 應用程式來說，這是一項中斷性變更：</span><span class="sxs-lookup"><span data-stu-id="38255-678">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="38255-679">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="38255-679">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="38255-680">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="38255-680">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="38255-681">針對需要使用 Libuv 的專案：</span><span class="sxs-lookup"><span data-stu-id="38255-681">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="38255-682">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="38255-682">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="38255-683">呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="38255-683">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="38255-684">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="38255-684">URL prefixes</span></span>

<span data-ttu-id="38255-685">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="38255-685">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="38255-686">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="38255-686">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="38255-687">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-687">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="38255-688">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-688">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="38255-689">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="38255-689">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="38255-690">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-690">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="38255-691">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="38255-691">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="38255-692">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-692">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="38255-693">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="38255-693">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="38255-694">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="38255-694">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="38255-695">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="38255-695">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="38255-696">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="38255-696">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="38255-697">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-697">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="38255-698">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="38255-698">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="38255-699">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="38255-699">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="38255-700">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="38255-700">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="38255-701">主機篩選</span><span class="sxs-lookup"><span data-stu-id="38255-701">Host filtering</span></span>

<span data-ttu-id="38255-702">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="38255-702">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="38255-703">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="38255-703">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="38255-704">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="38255-704">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="38255-705">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="38255-705">`Host` headers aren't validated.</span></span>

<span data-ttu-id="38255-706">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38255-706">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="38255-707">主機篩選中介軟體是由[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)（ASP.NET Core 2.1 或2.2）所包含的[HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件所提供。</span><span class="sxs-lookup"><span data-stu-id="38255-707">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="38255-708">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="38255-708">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="38255-709">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38255-709">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="38255-710">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="38255-710">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="38255-711">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="38255-711">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="38255-712">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="38255-712">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="38255-713">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="38255-713">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="38255-714">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="38255-714">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="38255-715">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="38255-715">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="38255-716">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="38255-716">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="38255-717">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="38255-717">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="38255-718">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="38255-718">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="38255-719">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="38255-719">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="38255-720">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="38255-720">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="38255-721">HTTPS</span><span class="sxs-lookup"><span data-stu-id="38255-721">HTTPS</span></span>
* <span data-ttu-id="38255-722">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="38255-722">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="38255-723">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="38255-723">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="38255-724">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-724">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="38255-725">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="38255-725">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="38255-726">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="38255-726">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="38255-727">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="38255-727">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="38255-728">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-728">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="38255-729">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="38255-729">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="38255-731">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="38255-731">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="38255-733">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="38255-733">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="38255-734">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-734">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="38255-735">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="38255-735">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="38255-736">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-736">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="38255-737">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="38255-737">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="38255-738">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="38255-738">A reverse proxy:</span></span>

* <span data-ttu-id="38255-739">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="38255-739">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="38255-740">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="38255-740">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="38255-741">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="38255-741">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="38255-742">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="38255-742">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="38255-743">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="38255-743">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="38255-744">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="38255-744">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="38255-745">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="38255-745">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="38255-746">[Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)套件包含在 AspNetCore 中。[應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="38255-746">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="38255-747">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-747">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="38255-748">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="38255-748">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="38255-749">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="38255-749">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

## <a name="kestrel-options"></a><span data-ttu-id="38255-750">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="38255-750">Kestrel options</span></span>

<span data-ttu-id="38255-751">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="38255-751">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="38255-752">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="38255-752">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="38255-753">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="38255-753">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="38255-754">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="38255-754">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="38255-755">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="38255-755">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="38255-756">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="38255-756">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="38255-757">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="38255-757">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="38255-758">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="38255-758">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="38255-759">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="38255-759">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="38255-760">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="38255-760">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="38255-761">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="38255-761">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="38255-762">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="38255-762">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="38255-763">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="38255-763">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="38255-764">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="38255-764">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="38255-765">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="38255-765">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="38255-766">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="38255-766">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="38255-767">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="38255-767">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="38255-768">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="38255-768">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="38255-769">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="38255-769">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="38255-770">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="38255-770">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="38255-771">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="38255-771">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="38255-772">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="38255-772">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="38255-773">如果速率低於下限值，則連線會逾時。寬限期是 Kestrel 提供給用戶端的時間量，以便將其傳送速率提高到下限值；在這段期間不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="38255-773">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="38255-774">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="38255-774">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="38255-775">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="38255-775">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="38255-776">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="38255-776">A minimum rate also applies to the response.</span></span> <span data-ttu-id="38255-777">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="38255-777">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="38255-778">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="38255-778">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            serverOptions.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

### <a name="request-headers-timeout"></a><span data-ttu-id="38255-779">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="38255-779">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="38255-780">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="38255-780">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="38255-781">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="38255-781">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="38255-782">同步 IO</span><span class="sxs-lookup"><span data-stu-id="38255-782">Synchronous IO</span></span>

<span data-ttu-id="38255-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> 控制是否允許要求與回應的同步 IO。</span><span class="sxs-lookup"><span data-stu-id="38255-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="38255-784">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="38255-784">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="38255-785">大量的封鎖同步 IO 作業會導致執行緒集區耗盡，這會使得應用程式沒有回應。</span><span class="sxs-lookup"><span data-stu-id="38255-785">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="38255-786">只有當使用不支援同步 IO 的程式庫時才啟用 `AllowSynchronousIO`。</span><span class="sxs-lookup"><span data-stu-id="38255-786">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="38255-787">下列範例會停用同步 IO：</span><span class="sxs-lookup"><span data-stu-id="38255-787">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="38255-788">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="38255-788">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="38255-789">端點組態</span><span class="sxs-lookup"><span data-stu-id="38255-789">Endpoint configuration</span></span>

<span data-ttu-id="38255-790">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="38255-790">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="38255-791">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="38255-791">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="38255-792">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="38255-792">Specify URLs using the:</span></span>

* <span data-ttu-id="38255-793">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="38255-793">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="38255-794">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="38255-794">`--urls` command-line argument.</span></span>
* <span data-ttu-id="38255-795">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="38255-795">`urls` host configuration key.</span></span>
* <span data-ttu-id="38255-796">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="38255-796">`UseUrls` extension method.</span></span>

<span data-ttu-id="38255-797">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="38255-797">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="38255-798">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000; http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="38255-798">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="38255-799">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="38255-799">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="38255-800">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="38255-800">A development certificate is created:</span></span>

* <span data-ttu-id="38255-801">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="38255-801">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="38255-802">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-802">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="38255-803">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-803">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="38255-804">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="38255-804">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="38255-805">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-805">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="38255-806">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="38255-806">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="38255-807">`KestrelServerOptions`配置</span><span class="sxs-lookup"><span data-stu-id="38255-807">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="38255-808">ConfigureEndpointDefaults （Action\<listenoptions 來 >）</span><span class="sxs-lookup"><span data-stu-id="38255-808">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="38255-809">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="38255-809">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="38255-810">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="38255-810">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="38255-811">ConfigureHttpsDefaults （Action\<HttpsConnectionAdapterOptions >）</span><span class="sxs-lookup"><span data-stu-id="38255-811">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="38255-812">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="38255-812">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="38255-813">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="38255-813">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="38255-814">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="38255-814">Configure(IConfiguration)</span></span>

<span data-ttu-id="38255-815">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="38255-815">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="38255-816">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="38255-816">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="38255-817">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="38255-817">ListenOptions.UseHttps</span></span>

<span data-ttu-id="38255-818">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-818">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="38255-819">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="38255-819">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="38255-820">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="38255-820">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="38255-821">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="38255-821">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="38255-822">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="38255-822">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="38255-823">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="38255-823">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="38255-824">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="38255-824">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="38255-825">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="38255-825">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="38255-826">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="38255-826">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="38255-827">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="38255-827">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="38255-828">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="38255-828">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="38255-829">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-829">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="38255-830">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="38255-830">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="38255-831">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-831">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="38255-832">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-832">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="38255-833">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-833">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="38255-834">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="38255-834">Supported configurations described next:</span></span>

* <span data-ttu-id="38255-835">無組態</span><span class="sxs-lookup"><span data-stu-id="38255-835">No configuration</span></span>
* <span data-ttu-id="38255-836">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="38255-836">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="38255-837">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="38255-837">Change the defaults in code</span></span>

<span data-ttu-id="38255-838">*無組態*</span><span class="sxs-lookup"><span data-stu-id="38255-838">*No configuration*</span></span>

<span data-ttu-id="38255-839">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="38255-839">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="38255-840">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="38255-840">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="38255-841">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="38255-841">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="38255-842">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="38255-842">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="38255-843">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="38255-843">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="38255-844">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="38255-844">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="38255-845">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="38255-845">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="38255-846">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-846">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="38255-847">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-847">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="38255-848">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="38255-848">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="38255-849">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="38255-849">Schema notes:</span></span>

* <span data-ttu-id="38255-850">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="38255-850">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="38255-851">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="38255-851">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="38255-852">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="38255-852">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="38255-853">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="38255-853">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="38255-854">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="38255-854">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="38255-855">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="38255-855">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="38255-856">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="38255-856">The `Certificate` section is optional.</span></span> <span data-ttu-id="38255-857">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="38255-857">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="38255-858">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="38255-858">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="38255-859">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-859">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="38255-860">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="38255-860">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="38255-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="38255-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="38255-862">`KestrelServerOptions.ConfigurationLoader`可以直接存取，以繼續逐一查看現有的載入器，例如所提供<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>的載入器。</span><span class="sxs-lookup"><span data-stu-id="38255-862">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="38255-863">每個端點的組態區段可用於 `Endpoint` 方法的選項，因此可讀取自訂組態。</span><span class="sxs-lookup"><span data-stu-id="38255-863">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="38255-864">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="38255-864">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="38255-865">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="38255-865">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="38255-866">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="38255-866">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="38255-867">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="38255-867">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="38255-868">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="38255-868">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="38255-869">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="38255-869">*Change the defaults in code*</span></span>

<span data-ttu-id="38255-870">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-870">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="38255-871">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="38255-871">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="38255-872">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="38255-872">*Kestrel support for SNI*</span></span>

<span data-ttu-id="38255-873">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="38255-873">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="38255-874">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-874">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="38255-875">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="38255-875">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="38255-876">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="38255-876">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="38255-877">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="38255-877">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="38255-878">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="38255-878">SNI support requires:</span></span>

* <span data-ttu-id="38255-879">在目標 framework `netcoreapp2.1`或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="38255-879">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="38255-880">在`net461`或更新版本上，會叫用回呼`name` ，但`null`一律為。</span><span class="sxs-lookup"><span data-stu-id="38255-880">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="38255-881">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="38255-881">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="38255-882">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="38255-882">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="38255-883">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-883">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="38255-884">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="38255-884">Bind to a TCP socket</span></span>

<span data-ttu-id="38255-885"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="38255-885">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
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
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="38255-886">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-886">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="38255-887">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="38255-887">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="38255-888">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="38255-888">Bind to a Unix socket</span></span>

<span data-ttu-id="38255-889">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="38255-889">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock");
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

### <a name="port-0"></a><span data-ttu-id="38255-890">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="38255-890">Port 0</span></span>

<span data-ttu-id="38255-891">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="38255-891">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="38255-892">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="38255-892">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="38255-893">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="38255-893">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="38255-894">限制</span><span class="sxs-lookup"><span data-stu-id="38255-894">Limitations</span></span>

<span data-ttu-id="38255-895">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="38255-895">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="38255-896">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="38255-896">`--urls` command-line argument</span></span>
* <span data-ttu-id="38255-897">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="38255-897">`urls` host configuration key</span></span>
* <span data-ttu-id="38255-898">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="38255-898">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="38255-899">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="38255-899">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="38255-900">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="38255-900">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="38255-901">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="38255-901">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="38255-902">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="38255-902">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="38255-903">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="38255-903">IIS endpoint configuration</span></span>

<span data-ttu-id="38255-904">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="38255-904">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="38255-905">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="38255-905">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="38255-906">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="38255-906">Transport configuration</span></span>

<span data-ttu-id="38255-907">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="38255-907">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="38255-908">對於升級到 2.1 且會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> 並相依於下列任一套件的 ASP.NET Core 2.0 應用程式來說，這是一項中斷性變更：</span><span class="sxs-lookup"><span data-stu-id="38255-908">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="38255-909">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="38255-909">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="38255-910">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="38255-910">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="38255-911">針對需要使用 Libuv 的專案：</span><span class="sxs-lookup"><span data-stu-id="38255-911">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="38255-912">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="38255-912">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="38255-913">呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="38255-913">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="38255-914">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="38255-914">URL prefixes</span></span>

<span data-ttu-id="38255-915">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="38255-915">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="38255-916">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="38255-916">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="38255-917">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="38255-917">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="38255-918">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-918">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="38255-919">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="38255-919">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="38255-920">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-920">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="38255-921">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="38255-921">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="38255-922">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-922">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="38255-923">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="38255-923">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="38255-924">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="38255-924">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="38255-925">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="38255-925">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="38255-926">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="38255-926">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="38255-927">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="38255-927">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="38255-928">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="38255-928">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="38255-929">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="38255-929">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="38255-930">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="38255-930">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="38255-931">主機篩選</span><span class="sxs-lookup"><span data-stu-id="38255-931">Host filtering</span></span>

<span data-ttu-id="38255-932">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="38255-932">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="38255-933">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="38255-933">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="38255-934">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="38255-934">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="38255-935">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="38255-935">`Host` headers aren't validated.</span></span>

<span data-ttu-id="38255-936">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38255-936">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="38255-937">主機篩選中介軟體是由[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)（ASP.NET Core 2.1 或2.2）所包含的[HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件所提供。</span><span class="sxs-lookup"><span data-stu-id="38255-937">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="38255-938">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="38255-938">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="38255-939">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38255-939">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="38255-940">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="38255-940">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="38255-941">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="38255-941">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="38255-942">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="38255-942">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="38255-943">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="38255-943">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="38255-944">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="38255-944">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="38255-945">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="38255-945">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="38255-946">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="38255-946">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="38255-947">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="38255-947">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="38255-948">其他資源</span><span class="sxs-lookup"><span data-stu-id="38255-948">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="38255-949">RFC 7230：Message Syntax and Routing (訊息語法和路由) 第 5.4 節：Host (主機)</span><span class="sxs-lookup"><span data-stu-id="38255-949">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
