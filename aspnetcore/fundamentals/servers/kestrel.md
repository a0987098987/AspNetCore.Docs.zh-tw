---
title: ASP.NET Core 中的 Kestrel 網頁伺服器實作
author: guardrex
description: 了解 Kestrel，這是 ASP.NET Core 的跨平台網頁伺服器。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bab751bc1453481a11114a7a8c0787fa5576e500
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2019
ms.locfileid: "73427065"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="74f5c-103">ASP.NET Core 中的 Kestrel 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="74f5c-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="74f5c-104">由[Tom 作者: dykstra](https://github.com/tdykstra)、 [Chris Ross](https://github.com/Tratcher)、 [Stephen Halter](https://twitter.com/halter73)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="74f5c-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="74f5c-105">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="74f5c-106">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="74f5c-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="74f5c-107">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="74f5c-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="74f5c-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="74f5c-108">HTTPS</span></span>
* <span data-ttu-id="74f5c-109">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="74f5c-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="74f5c-110">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="74f5c-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="74f5c-111">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="74f5c-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="74f5c-112">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="74f5c-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="74f5c-113">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="74f5c-114">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74f5c-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="74f5c-115">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="74f5c-115">HTTP/2 support</span></span>

<span data-ttu-id="74f5c-116">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="74f5c-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="74f5c-117">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="74f5c-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="74f5c-118">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="74f5c-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="74f5c-119">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="74f5c-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="74f5c-120">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="74f5c-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="74f5c-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="74f5c-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="74f5c-122">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="74f5c-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="74f5c-123">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="74f5c-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="74f5c-124">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="74f5c-125">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="74f5c-126">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="74f5c-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="74f5c-127">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="74f5c-128">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="74f5c-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="74f5c-129">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="74f5c-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="74f5c-130">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="74f5c-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="74f5c-131">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="74f5c-132">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="74f5c-133">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="74f5c-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="74f5c-135">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="74f5c-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="74f5c-137">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="74f5c-138">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="74f5c-139">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="74f5c-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="74f5c-140">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="74f5c-141">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="74f5c-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="74f5c-142">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="74f5c-142">A reverse proxy:</span></span>

* <span data-ttu-id="74f5c-143">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="74f5c-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="74f5c-144">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="74f5c-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="74f5c-145">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="74f5c-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="74f5c-146">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="74f5c-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="74f5c-147">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="74f5c-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="74f5c-148">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="74f5c-149">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="74f5c-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="74f5c-150">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="74f5c-151">在*Program.cs*中，應用程式會呼叫 `ConfigureWebHostDefaults`，這會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="74f5c-151">In *Program.cs*, the app calls `ConfigureWebHostDefaults`, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=8)]

<span data-ttu-id="74f5c-152">如需建立主機的詳細資訊，請參閱 <xref:fundamentals/host/generic-host#set-up-a-host>的*設定主機*和*預設*產生器設定章節。</span><span class="sxs-lookup"><span data-stu-id="74f5c-152">For more information on building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="74f5c-153">若要在呼叫 `ConfigureWebHostDefaults` 之後提供額外的設定，請使用 `ConfigureKestrel`：</span><span class="sxs-lookup"><span data-stu-id="74f5c-153">To provide additional configuration after calling `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="74f5c-154">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="74f5c-154">Kestrel options</span></span>

<span data-ttu-id="74f5c-155">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="74f5c-156">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="74f5c-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="74f5c-157">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="74f5c-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="74f5c-158">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="74f5c-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="74f5c-159">Kestrel 選項（在C#下列範例的程式碼中設定）也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-159">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="74f5c-160">例如，檔案設定提供者可以從*appsettings*或 Appsettings 載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="74f5c-160">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    },
    "DisableStringReuse": true
  }
}
```

<span data-ttu-id="74f5c-161">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="74f5c-161">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="74f5c-162">在 `Startup.ConfigureServices`中設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="74f5c-162">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="74f5c-163">將 `IConfiguration` 的實例插入 `Startup` 類別中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-163">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="74f5c-164">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="74f5c-164">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="74f5c-165">在 `Startup.ConfigureServices` 中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-165">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="74f5c-166">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="74f5c-166">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="74f5c-167">在*Program.cs*中，將設定的 [`Kestrel`] 區段載入 Kestrel 的設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-167">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IHostBuilder CreateHostBuilder(string[] args) =>
      Host.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .ConfigureWebHostDefaults(webBuilder =>
          {
              webBuilder.UseStartup<Startup>();
          });
  ```

<span data-ttu-id="74f5c-168">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-168">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="74f5c-169">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="74f5c-169">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="74f5c-170">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-170">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="74f5c-171">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="74f5c-171">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="74f5c-172">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-172">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="74f5c-173">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="74f5c-173">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="74f5c-174">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-174">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="74f5c-175">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-175">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="74f5c-176">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-176">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="74f5c-177">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-177">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="74f5c-178">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="74f5c-178">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="74f5c-179">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="74f5c-179">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="74f5c-180">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="74f5c-180">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="74f5c-181">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-181">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="74f5c-182">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="74f5c-182">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="74f5c-183">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="74f5c-183">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="74f5c-184">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-184">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="74f5c-185">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="74f5c-185">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="74f5c-186">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="74f5c-186">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="74f5c-187">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="74f5c-187">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="74f5c-188">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="74f5c-188">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="74f5c-189">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="74f5c-189">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="74f5c-190">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="74f5c-190">A minimum rate also applies to the response.</span></span> <span data-ttu-id="74f5c-191">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="74f5c-191">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="74f5c-192">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="74f5c-192">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="74f5c-193">覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="74f5c-193">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="74f5c-194">因為通訊協定對要求多工的支援，所以 HTTP/2 一般不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> 不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-194">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="74f5c-195">不過，<xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> 仍存在 HTTP/2 要求的 `HttpContext.Features`您仍能透過將 `IHttpMinRequestBodyDataRateFeature.MinDataRate` 設定為 `null` (即使是針對 HTTP/2 要求)，以個別要求基礎來「完全停用」讀取素率限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-195">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="74f5c-196">嘗試讀取 `IHttpMinRequestBodyDataRateFeature.MinDataRate` 或嘗試將它設定為 `null` 以外的值將會導致擲回 `NotSupportedException` (假設要求是 HTTP/2 要求)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-196">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="74f5c-197">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="74f5c-197">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="74f5c-198">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="74f5c-198">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="74f5c-199">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-199">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="74f5c-200">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="74f5c-200">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="74f5c-201">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-201">Maximum streams per connection</span></span>

<span data-ttu-id="74f5c-202">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="74f5c-202">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="74f5c-203">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="74f5c-203">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="74f5c-204">預設值為 100。</span><span class="sxs-lookup"><span data-stu-id="74f5c-204">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="74f5c-205">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="74f5c-205">Header table size</span></span>

<span data-ttu-id="74f5c-206">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="74f5c-206">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="74f5c-207">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="74f5c-207">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="74f5c-208">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-208">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="74f5c-209">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="74f5c-209">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="74f5c-210">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-210">Maximum frame size</span></span>

<span data-ttu-id="74f5c-211">`Http2.MaxFrameSize` 表示伺服器所接收或傳送之 HTTP/2 連接框架承載的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-211">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="74f5c-212">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="74f5c-212">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="74f5c-213">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-213">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="74f5c-214">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-214">Maximum request header size</span></span>

<span data-ttu-id="74f5c-215">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-215">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="74f5c-216">這項限制適用于其壓縮和未壓縮標記法中的名稱和值。</span><span class="sxs-lookup"><span data-stu-id="74f5c-216">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="74f5c-217">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-217">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="74f5c-218">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="74f5c-218">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="74f5c-219">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="74f5c-219">Initial connection window size</span></span>

<span data-ttu-id="74f5c-220">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-220">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="74f5c-221">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-221">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="74f5c-222">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-222">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="74f5c-223">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-223">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="74f5c-224">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="74f5c-224">Initial stream window size</span></span>

<span data-ttu-id="74f5c-225">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-225">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="74f5c-226">要求也皆受 `Http2.InitialConnectionWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-226">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="74f5c-227">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-227">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="74f5c-228">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-228">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="74f5c-229">同步 IO</span><span class="sxs-lookup"><span data-stu-id="74f5c-229">Synchronous IO</span></span>

<span data-ttu-id="74f5c-230"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> 控制是否允許要求與回應的同步 IO。</span><span class="sxs-lookup"><span data-stu-id="74f5c-230"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="74f5c-231">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-231">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="74f5c-232">大量的封鎖同步 IO 作業會導致執行緒集區耗盡，這會使得應用程式沒有回應。</span><span class="sxs-lookup"><span data-stu-id="74f5c-232">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="74f5c-233">只有當使用不支援同步 IO 的程式庫時才啟用 `AllowSynchronousIO`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-233">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="74f5c-234">下列範例會啟用同步 IO：</span><span class="sxs-lookup"><span data-stu-id="74f5c-234">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="74f5c-235">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="74f5c-235">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="74f5c-236">端點組態</span><span class="sxs-lookup"><span data-stu-id="74f5c-236">Endpoint configuration</span></span>

<span data-ttu-id="74f5c-237">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="74f5c-237">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="74f5c-238">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="74f5c-238">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="74f5c-239">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="74f5c-239">Specify URLs using the:</span></span>

* <span data-ttu-id="74f5c-240">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="74f5c-240">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="74f5c-241">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="74f5c-241">`--urls` command-line argument.</span></span>
* <span data-ttu-id="74f5c-242">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="74f5c-242">`urls` host configuration key.</span></span>
* <span data-ttu-id="74f5c-243">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="74f5c-243">`UseUrls` extension method.</span></span>

<span data-ttu-id="74f5c-244">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-244">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="74f5c-245">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000; http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-245">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="74f5c-246">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-246">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="74f5c-247">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="74f5c-247">A development certificate is created:</span></span>

* <span data-ttu-id="74f5c-248">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="74f5c-248">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="74f5c-249">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-249">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="74f5c-250">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-250">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="74f5c-251">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-251">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="74f5c-252">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-252">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="74f5c-253">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-253">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="74f5c-254">`KestrelServerOptions` 設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-254">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="74f5c-255">ConfigureEndpointDefaults （Action\<Listenoptions 來 >）</span><span class="sxs-lookup"><span data-stu-id="74f5c-255">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="74f5c-256">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-256">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="74f5c-257">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-257">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="74f5c-258">ConfigureHttpsDefaults （Action\<HttpsConnectionAdapterOptions >）</span><span class="sxs-lookup"><span data-stu-id="74f5c-258">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="74f5c-259">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-259">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="74f5c-260">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-260">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        // certificate is an X509Certificate2
        listenOptions.ServerCertificate = certificate;
    });
});
```

### <a name="configureiconfiguration"></a><span data-ttu-id="74f5c-261">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="74f5c-261">Configure(IConfiguration)</span></span>

<span data-ttu-id="74f5c-262">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-262">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="74f5c-263">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="74f5c-263">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="74f5c-264">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="74f5c-264">ListenOptions.UseHttps</span></span>

<span data-ttu-id="74f5c-265">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-265">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="74f5c-266">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="74f5c-266">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="74f5c-267">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-267">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="74f5c-268">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="74f5c-268">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="74f5c-269">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="74f5c-269">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="74f5c-270">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="74f5c-270">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="74f5c-271">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="74f5c-271">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="74f5c-272">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-272">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="74f5c-273">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-273">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="74f5c-274">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="74f5c-274">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="74f5c-275">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="74f5c-275">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="74f5c-276">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-276">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="74f5c-277">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="74f5c-277">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="74f5c-278">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-278">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="74f5c-279">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-279">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="74f5c-280">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-280">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="74f5c-281">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="74f5c-281">Supported configurations described next:</span></span>

* <span data-ttu-id="74f5c-282">無組態</span><span class="sxs-lookup"><span data-stu-id="74f5c-282">No configuration</span></span>
* <span data-ttu-id="74f5c-283">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="74f5c-283">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="74f5c-284">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="74f5c-284">Change the defaults in code</span></span>

<span data-ttu-id="74f5c-285">*無組態*</span><span class="sxs-lookup"><span data-stu-id="74f5c-285">*No configuration*</span></span>

<span data-ttu-id="74f5c-286">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-286">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="74f5c-287">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="74f5c-287">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="74f5c-288">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-288">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="74f5c-289">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="74f5c-289">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="74f5c-290">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="74f5c-290">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="74f5c-291">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="74f5c-291">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="74f5c-292">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-292">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="74f5c-293">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-293">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="74f5c-294">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-294">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="74f5c-295">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="74f5c-295">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="74f5c-296">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="74f5c-296">Schema notes:</span></span>

* <span data-ttu-id="74f5c-297">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="74f5c-297">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="74f5c-298">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="74f5c-298">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="74f5c-299">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="74f5c-299">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="74f5c-300">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="74f5c-300">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="74f5c-301">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-301">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="74f5c-302">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="74f5c-302">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="74f5c-303">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="74f5c-303">The `Certificate` section is optional.</span></span> <span data-ttu-id="74f5c-304">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="74f5c-304">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="74f5c-305">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="74f5c-305">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="74f5c-306">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-306">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="74f5c-307">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="74f5c-307">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="74f5c-308">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-308">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
webBuilder.UseKestrel((context, serverOptions) =>
{
    serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
        .Endpoint("HTTPS", listenOptions =>
        {
            listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
        });
});
```

<span data-ttu-id="74f5c-309">`KestrelServerOptions.ConfigurationLoader` 可以直接存取，以繼續逐一查看現有的載入器，例如 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所提供的載入器。</span><span class="sxs-lookup"><span data-stu-id="74f5c-309">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="74f5c-310">每個端點的 [設定] 區段都可以在 `Endpoint` 方法的選項中使用，如此一來，就可以讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-310">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="74f5c-311">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="74f5c-311">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="74f5c-312">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-312">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="74f5c-313">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="74f5c-313">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="74f5c-314">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="74f5c-314">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="74f5c-315">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="74f5c-315">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="74f5c-316">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="74f5c-316">*Change the defaults in code*</span></span>

<span data-ttu-id="74f5c-317">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-317">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="74f5c-318">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="74f5c-318">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
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
```

<span data-ttu-id="74f5c-319">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="74f5c-319">*Kestrel support for SNI*</span></span>

<span data-ttu-id="74f5c-320">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="74f5c-320">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="74f5c-321">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-321">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="74f5c-322">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="74f5c-322">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="74f5c-323">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="74f5c-323">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="74f5c-324">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-324">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="74f5c-325">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="74f5c-325">SNI support requires:</span></span>

* <span data-ttu-id="74f5c-326">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-326">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="74f5c-327">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律會 `null`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-327">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="74f5c-328">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-328">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="74f5c-329">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-329">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="74f5c-330">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-330">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
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
});
```

### <a name="connection-logging"></a><span data-ttu-id="74f5c-331">連接記錄</span><span class="sxs-lookup"><span data-stu-id="74f5c-331">Connection logging</span></span>

<span data-ttu-id="74f5c-332">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>，針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="74f5c-332">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="74f5c-333">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="74f5c-333">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="74f5c-334">如果 `UseConnectionLogging` 放在 `UseHttps`之前，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="74f5c-334">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="74f5c-335">如果 `UseConnectionLogging` 放在 `UseHttps`之後，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="74f5c-335">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="74f5c-336">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="74f5c-336">Bind to a TCP socket</span></span>

<span data-ttu-id="74f5c-337"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-337">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="74f5c-338">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-338">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="74f5c-339">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="74f5c-339">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="74f5c-340">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="74f5c-340">Bind to a Unix socket</span></span>

<span data-ttu-id="74f5c-341">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="74f5c-341">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="74f5c-342">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="74f5c-342">Port 0</span></span>

<span data-ttu-id="74f5c-343">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-343">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="74f5c-344">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="74f5c-344">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="74f5c-345">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="74f5c-345">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="74f5c-346">限制</span><span class="sxs-lookup"><span data-stu-id="74f5c-346">Limitations</span></span>

<span data-ttu-id="74f5c-347">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="74f5c-347">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="74f5c-348">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="74f5c-348">`--urls` command-line argument</span></span>
* <span data-ttu-id="74f5c-349">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="74f5c-349">`urls` host configuration key</span></span>
* <span data-ttu-id="74f5c-350">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="74f5c-350">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="74f5c-351">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-351">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="74f5c-352">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="74f5c-352">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="74f5c-353">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-353">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="74f5c-354">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="74f5c-354">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="74f5c-355">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="74f5c-355">IIS endpoint configuration</span></span>

<span data-ttu-id="74f5c-356">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-356">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="74f5c-357">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="74f5c-357">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="74f5c-358">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="74f5c-358">ListenOptions.Protocols</span></span>

<span data-ttu-id="74f5c-359">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-359">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="74f5c-360">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="74f5c-360">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="74f5c-361">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="74f5c-361">`HttpProtocols` enum value</span></span> | <span data-ttu-id="74f5c-362">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="74f5c-362">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="74f5c-363">僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="74f5c-363">HTTP/1.1 only.</span></span> <span data-ttu-id="74f5c-364">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-364">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="74f5c-365">僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="74f5c-365">HTTP/2 only.</span></span> <span data-ttu-id="74f5c-366">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-366">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="74f5c-367">HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="74f5c-367">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="74f5c-368">HTTP/2 要求用戶端選取 TLS[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)交握中的 HTTP/2;否則，連接預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="74f5c-368">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="74f5c-369">任何端點的預設 `ListenOptions.Protocols` 值都會 `HttpProtocols.Http1AndHttp2`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-369">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="74f5c-370">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="74f5c-370">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="74f5c-371">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="74f5c-371">TLS version 1.2 or later</span></span>
* <span data-ttu-id="74f5c-372">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="74f5c-372">Renegotiation disabled</span></span>
* <span data-ttu-id="74f5c-373">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="74f5c-373">Compression disabled</span></span>
* <span data-ttu-id="74f5c-374">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="74f5c-374">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="74f5c-375">橢圓曲線 Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 最小 224 個位元</span><span class="sxs-lookup"><span data-stu-id="74f5c-375">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="74f5c-376">有限欄位 Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 最小 2048 個位元</span><span class="sxs-lookup"><span data-stu-id="74f5c-376">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="74f5c-377">加密套件未列於封鎖清單中</span><span class="sxs-lookup"><span data-stu-id="74f5c-377">Cipher suite not blacklisted</span></span>

<span data-ttu-id="74f5c-378">預設支援 `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; 與 P-256 橢圓曲線 &lbrack;`FIPS186`&rbrack;。</span><span class="sxs-lookup"><span data-stu-id="74f5c-378">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="74f5c-379">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="74f5c-379">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="74f5c-380">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="74f5c-380">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="74f5c-381">如有需要，請使用連線中介軟體，針對特定的加密依據每個連線來篩選 TLS 交握。</span><span class="sxs-lookup"><span data-stu-id="74f5c-381">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="74f5c-382">下列範例會針對應用程式不支援的任何加密演算法擲回 <xref:System.NotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="74f5c-382">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="74f5c-383">或者，定義 ITlsHandshakeFeature，並將[CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm)與可接受的加密套件清單進行比較。</span><span class="sxs-lookup"><span data-stu-id="74f5c-383">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="74f5c-384">不會使用加密來搭配[CipherAlgorithmType。 Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher 演算法。</span><span class="sxs-lookup"><span data-stu-id="74f5c-384">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseTlsFilter();
    });
});
```

```csharp
using System;
using System.Security.Authentication;
using Microsoft.AspNetCore.Connections.Features;

namespace Microsoft.AspNetCore.Connections
{
    public static class TlsFilterConnectionMiddlewareExtensions
    {
        public static IConnectionBuilder UseTlsFilter(
            this IConnectionBuilder builder)
        {
            return builder.Use((connection, next) =>
            {
                var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

                if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
                {
                    throw new NotSupportedException("Prohibited cipher: " +
                        tlsFeature.CipherAlgorithm);
                }

                return next();
            });
        }
    }
}
```

<span data-ttu-id="74f5c-385">您也可以透過 <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda 來設定連接篩選：</span><span class="sxs-lookup"><span data-stu-id="74f5c-385">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

```csharp
// using System;
// using System.Net;
// using System.Security.Authentication;
// using Microsoft.AspNetCore.Connections;
// using Microsoft.AspNetCore.Connections.Features;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.Use((context, next) =>
        {
            var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

            if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
            {
                throw new NotSupportedException(
                    $"Prohibited cipher: {tlsFeature.CipherAlgorithm}");
            }

            return next();
        });
    });
});
```

<span data-ttu-id="74f5c-386">在 Linux 上，您可以使用 <xref:System.Net.Security.CipherSuitesPolicy>，依據每個連線來篩選 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="74f5c-386">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

```csharp
// using System.Net.Security;
// using Microsoft.AspNetCore.Hosting;
// using Microsoft.AspNetCore.Server.Kestrel.Core;
// using Microsoft.Extensions.DependencyInjection;
// using Microsoft.Extensions.Hosting;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.OnAuthenticate = (context, sslOptions) =>
        {
            sslOptions.CipherSuitesPolicy = new CipherSuitesPolicy(
                new[]
                {
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
                    // ...
                });
        };
    });
});
```

<span data-ttu-id="74f5c-387">從設定進行通訊協定設定</span><span class="sxs-lookup"><span data-stu-id="74f5c-387">*Set the protocol from configuration*</span></span>

<span data-ttu-id="74f5c-388">`CreateDefaultBuilder` 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-388">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="74f5c-389">下列*appsettings*範例會建立 HTTP/1.1 作為所有端點的預設連接通訊協定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-389">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="74f5c-390">下列*appsettings*範例會建立特定端點的 HTTP/1.1 連接通訊協定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-390">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="74f5c-391">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="74f5c-391">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="74f5c-392">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="74f5c-392">Transport configuration</span></span>

<span data-ttu-id="74f5c-393">針對需要使用 Libuv （<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>）的專案：</span><span class="sxs-lookup"><span data-stu-id="74f5c-393">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="74f5c-394">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="74f5c-394">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="74f5c-395">在 `IWebHostBuilder` 上呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="74f5c-395">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="74f5c-396">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="74f5c-396">URL prefixes</span></span>

<span data-ttu-id="74f5c-397">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="74f5c-397">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="74f5c-398">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="74f5c-398">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="74f5c-399">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-399">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="74f5c-400">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-400">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="74f5c-401">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="74f5c-401">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="74f5c-402">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-402">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="74f5c-403">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="74f5c-403">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="74f5c-404">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-404">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="74f5c-405">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="74f5c-405">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="74f5c-406">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="74f5c-406">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="74f5c-407">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-407">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="74f5c-408">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-408">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="74f5c-409">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-409">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="74f5c-410">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="74f5c-410">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="74f5c-411">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="74f5c-411">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="74f5c-412">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="74f5c-412">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="74f5c-413">主機篩選</span><span class="sxs-lookup"><span data-stu-id="74f5c-413">Host filtering</span></span>

<span data-ttu-id="74f5c-414">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="74f5c-414">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="74f5c-415">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="74f5c-415">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="74f5c-416">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="74f5c-416">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="74f5c-417">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-417">`Host` headers aren't validated.</span></span>

<span data-ttu-id="74f5c-418">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="74f5c-418">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="74f5c-419">主機篩選中介軟體是由[AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件提供，這是針對 ASP.NET Core 應用程式而隱含提供的。</span><span class="sxs-lookup"><span data-stu-id="74f5c-419">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="74f5c-420">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="74f5c-420">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="74f5c-421">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="74f5c-421">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="74f5c-422">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="74f5c-422">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="74f5c-423">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="74f5c-423">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="74f5c-424">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="74f5c-424">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="74f5c-425">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="74f5c-425">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="74f5c-426">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="74f5c-426">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="74f5c-427">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-427">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="74f5c-428">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-428">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="74f5c-429">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="74f5c-429">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="74f5c-430">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-430">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="74f5c-431">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="74f5c-431">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="74f5c-432">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="74f5c-432">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="74f5c-433">HTTPS</span><span class="sxs-lookup"><span data-stu-id="74f5c-433">HTTPS</span></span>
* <span data-ttu-id="74f5c-434">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="74f5c-434">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="74f5c-435">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="74f5c-435">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="74f5c-436">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="74f5c-436">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="74f5c-437">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="74f5c-437">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="74f5c-438">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-438">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="74f5c-439">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74f5c-439">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="74f5c-440">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="74f5c-440">HTTP/2 support</span></span>

<span data-ttu-id="74f5c-441">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="74f5c-441">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="74f5c-442">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="74f5c-442">Operating system&dagger;</span></span>
  * <span data-ttu-id="74f5c-443">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="74f5c-443">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="74f5c-444">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="74f5c-444">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="74f5c-445">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="74f5c-445">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="74f5c-446">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="74f5c-446">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="74f5c-447">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="74f5c-447">TLS 1.2 or later connection</span></span>

<span data-ttu-id="74f5c-448">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="74f5c-448">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="74f5c-449">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-449">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="74f5c-450">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-450">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="74f5c-451">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="74f5c-451">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="74f5c-452">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-452">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="74f5c-453">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="74f5c-453">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="74f5c-454">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="74f5c-454">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="74f5c-455">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="74f5c-455">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="74f5c-456">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-456">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="74f5c-457">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-457">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="74f5c-458">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="74f5c-458">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="74f5c-460">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="74f5c-460">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="74f5c-462">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-462">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="74f5c-463">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-463">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="74f5c-464">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="74f5c-464">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="74f5c-465">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-465">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="74f5c-466">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="74f5c-466">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="74f5c-467">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="74f5c-467">A reverse proxy:</span></span>

* <span data-ttu-id="74f5c-468">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="74f5c-468">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="74f5c-469">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="74f5c-469">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="74f5c-470">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="74f5c-470">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="74f5c-471">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="74f5c-471">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="74f5c-472">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="74f5c-472">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="74f5c-473">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-473">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="74f5c-474">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="74f5c-474">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="74f5c-475">[Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)套件包含在 AspNetCore 中。[應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-475">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="74f5c-476">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-476">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="74f5c-477">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="74f5c-477">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="74f5c-478">如需 `CreateDefaultBuilder` 和建立主機的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host> 的*設定主機*一節。</span><span class="sxs-lookup"><span data-stu-id="74f5c-478">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="74f5c-479">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請使用 `ConfigureKestrel`：</span><span class="sxs-lookup"><span data-stu-id="74f5c-479">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="74f5c-480">如果應用程式未呼叫 `CreateDefaultBuilder` 來設定主機，請在呼叫 `ConfigureKestrel` **之前**，先呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="74f5c-480">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="74f5c-481">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="74f5c-481">Kestrel options</span></span>

<span data-ttu-id="74f5c-482">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-482">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="74f5c-483">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="74f5c-483">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="74f5c-484">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="74f5c-484">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="74f5c-485">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="74f5c-485">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="74f5c-486">Kestrel 選項（在C#下列範例的程式碼中設定）也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-486">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="74f5c-487">例如，檔案設定提供者可以從*appsettings*或 Appsettings 載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="74f5c-487">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="74f5c-488">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="74f5c-488">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="74f5c-489">在 `Startup.ConfigureServices`中設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="74f5c-489">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="74f5c-490">將 `IConfiguration` 的實例插入 `Startup` 類別中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-490">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="74f5c-491">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="74f5c-491">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="74f5c-492">在 `Startup.ConfigureServices` 中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-492">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="74f5c-493">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="74f5c-493">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="74f5c-494">在*Program.cs*中，將設定的 [`Kestrel`] 區段載入 Kestrel 的設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-494">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="74f5c-495">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-495">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="74f5c-496">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="74f5c-496">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="74f5c-497">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-497">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="74f5c-498">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="74f5c-498">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="74f5c-499">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-499">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="74f5c-500">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="74f5c-500">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="74f5c-501">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-501">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="74f5c-502">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-502">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="74f5c-503">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-503">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="74f5c-504">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-504">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="74f5c-505">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="74f5c-505">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="74f5c-506">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="74f5c-506">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="74f5c-507">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="74f5c-507">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="74f5c-508">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-508">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="74f5c-509">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="74f5c-509">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="74f5c-510">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="74f5c-510">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="74f5c-511">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-511">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="74f5c-512">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="74f5c-512">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="74f5c-513">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="74f5c-513">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="74f5c-514">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="74f5c-514">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="74f5c-515">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="74f5c-515">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="74f5c-516">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="74f5c-516">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="74f5c-517">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="74f5c-517">A minimum rate also applies to the response.</span></span> <span data-ttu-id="74f5c-518">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="74f5c-518">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="74f5c-519">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="74f5c-519">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="74f5c-520">覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="74f5c-520">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="74f5c-521">因為通訊協定對要求多工的支援，所以 HTTP/2 不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的所有速率功能都不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-521">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="74f5c-522">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="74f5c-522">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="74f5c-523">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="74f5c-523">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="74f5c-524">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-524">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="74f5c-525">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="74f5c-525">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="74f5c-526">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-526">Maximum streams per connection</span></span>

<span data-ttu-id="74f5c-527">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="74f5c-527">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="74f5c-528">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="74f5c-528">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="74f5c-529">預設值為 100。</span><span class="sxs-lookup"><span data-stu-id="74f5c-529">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="74f5c-530">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="74f5c-530">Header table size</span></span>

<span data-ttu-id="74f5c-531">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="74f5c-531">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="74f5c-532">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="74f5c-532">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="74f5c-533">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-533">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="74f5c-534">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="74f5c-534">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="74f5c-535">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-535">Maximum frame size</span></span>

<span data-ttu-id="74f5c-536">`Http2.MaxFrameSize` 表示要接收的 HTTP/2 連線框架承載大小上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-536">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="74f5c-537">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="74f5c-537">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="74f5c-538">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-538">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="74f5c-539">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-539">Maximum request header size</span></span>

<span data-ttu-id="74f5c-540">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-540">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="74f5c-541">此限制皆共同套用至已壓縮及未壓縮代表中的名稱與值。</span><span class="sxs-lookup"><span data-stu-id="74f5c-541">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="74f5c-542">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-542">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="74f5c-543">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="74f5c-543">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="74f5c-544">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="74f5c-544">Initial connection window size</span></span>

<span data-ttu-id="74f5c-545">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-545">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="74f5c-546">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-546">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="74f5c-547">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-547">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="74f5c-548">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-548">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="74f5c-549">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="74f5c-549">Initial stream window size</span></span>

<span data-ttu-id="74f5c-550">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-550">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="74f5c-551">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-551">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="74f5c-552">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-552">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="74f5c-553">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-553">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="74f5c-554">同步 IO</span><span class="sxs-lookup"><span data-stu-id="74f5c-554">Synchronous IO</span></span>

<span data-ttu-id="74f5c-555"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> 控制是否允許要求與回應的同步 IO。</span><span class="sxs-lookup"><span data-stu-id="74f5c-555"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="74f5c-556">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-556">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="74f5c-557">大量的封鎖同步 IO 作業會導致執行緒集區耗盡，這會使得應用程式沒有回應。</span><span class="sxs-lookup"><span data-stu-id="74f5c-557">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="74f5c-558">只有當使用不支援同步 IO 的程式庫時才啟用 `AllowSynchronousIO`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-558">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="74f5c-559">下列範例會啟用同步 IO：</span><span class="sxs-lookup"><span data-stu-id="74f5c-559">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="74f5c-560">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="74f5c-560">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="74f5c-561">端點組態</span><span class="sxs-lookup"><span data-stu-id="74f5c-561">Endpoint configuration</span></span>

<span data-ttu-id="74f5c-562">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="74f5c-562">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="74f5c-563">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="74f5c-563">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="74f5c-564">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="74f5c-564">Specify URLs using the:</span></span>

* <span data-ttu-id="74f5c-565">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="74f5c-565">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="74f5c-566">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="74f5c-566">`--urls` command-line argument.</span></span>
* <span data-ttu-id="74f5c-567">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="74f5c-567">`urls` host configuration key.</span></span>
* <span data-ttu-id="74f5c-568">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="74f5c-568">`UseUrls` extension method.</span></span>

<span data-ttu-id="74f5c-569">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-569">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="74f5c-570">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000; http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-570">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="74f5c-571">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-571">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="74f5c-572">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="74f5c-572">A development certificate is created:</span></span>

* <span data-ttu-id="74f5c-573">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="74f5c-573">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="74f5c-574">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-574">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="74f5c-575">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-575">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="74f5c-576">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-576">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="74f5c-577">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-577">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="74f5c-578">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-578">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="74f5c-579">`KestrelServerOptions` 設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-579">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="74f5c-580">ConfigureEndpointDefaults （Action\<Listenoptions 來 >）</span><span class="sxs-lookup"><span data-stu-id="74f5c-580">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="74f5c-581">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-581">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="74f5c-582">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-582">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="74f5c-583">ConfigureHttpsDefaults （Action\<HttpsConnectionAdapterOptions >）</span><span class="sxs-lookup"><span data-stu-id="74f5c-583">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="74f5c-584">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-584">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="74f5c-585">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-585">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="74f5c-586">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="74f5c-586">Configure(IConfiguration)</span></span>

<span data-ttu-id="74f5c-587">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-587">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="74f5c-588">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="74f5c-588">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="74f5c-589">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="74f5c-589">ListenOptions.UseHttps</span></span>

<span data-ttu-id="74f5c-590">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-590">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="74f5c-591">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="74f5c-591">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="74f5c-592">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-592">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="74f5c-593">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="74f5c-593">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="74f5c-594">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="74f5c-594">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="74f5c-595">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="74f5c-595">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="74f5c-596">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="74f5c-596">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="74f5c-597">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-597">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="74f5c-598">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-598">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="74f5c-599">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="74f5c-599">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="74f5c-600">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="74f5c-600">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="74f5c-601">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-601">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="74f5c-602">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="74f5c-602">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="74f5c-603">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-603">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="74f5c-604">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-604">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="74f5c-605">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-605">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="74f5c-606">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="74f5c-606">Supported configurations described next:</span></span>

* <span data-ttu-id="74f5c-607">無組態</span><span class="sxs-lookup"><span data-stu-id="74f5c-607">No configuration</span></span>
* <span data-ttu-id="74f5c-608">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="74f5c-608">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="74f5c-609">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="74f5c-609">Change the defaults in code</span></span>

<span data-ttu-id="74f5c-610">*無組態*</span><span class="sxs-lookup"><span data-stu-id="74f5c-610">*No configuration*</span></span>

<span data-ttu-id="74f5c-611">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-611">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="74f5c-612">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="74f5c-612">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="74f5c-613">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-613">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="74f5c-614">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="74f5c-614">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="74f5c-615">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="74f5c-615">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="74f5c-616">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="74f5c-616">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="74f5c-617">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-617">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="74f5c-618">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-618">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="74f5c-619">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-619">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="74f5c-620">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="74f5c-620">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="74f5c-621">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="74f5c-621">Schema notes:</span></span>

* <span data-ttu-id="74f5c-622">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="74f5c-622">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="74f5c-623">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="74f5c-623">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="74f5c-624">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="74f5c-624">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="74f5c-625">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="74f5c-625">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="74f5c-626">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-626">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="74f5c-627">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="74f5c-627">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="74f5c-628">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="74f5c-628">The `Certificate` section is optional.</span></span> <span data-ttu-id="74f5c-629">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="74f5c-629">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="74f5c-630">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="74f5c-630">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="74f5c-631">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-631">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="74f5c-632">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="74f5c-632">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="74f5c-633">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-633">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="74f5c-634">`KestrelServerOptions.ConfigurationLoader` 可以直接存取，以繼續逐一查看現有的載入器，例如 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所提供的載入器。</span><span class="sxs-lookup"><span data-stu-id="74f5c-634">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="74f5c-635">每個端點的 [設定] 區段都可以在 `Endpoint` 方法的選項中使用，如此一來，就可以讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-635">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="74f5c-636">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="74f5c-636">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="74f5c-637">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-637">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="74f5c-638">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="74f5c-638">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="74f5c-639">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="74f5c-639">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="74f5c-640">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="74f5c-640">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="74f5c-641">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="74f5c-641">*Change the defaults in code*</span></span>

<span data-ttu-id="74f5c-642">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-642">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="74f5c-643">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="74f5c-643">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="74f5c-644">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="74f5c-644">*Kestrel support for SNI*</span></span>

<span data-ttu-id="74f5c-645">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="74f5c-645">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="74f5c-646">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-646">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="74f5c-647">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="74f5c-647">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="74f5c-648">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="74f5c-648">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="74f5c-649">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-649">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="74f5c-650">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="74f5c-650">SNI support requires:</span></span>

* <span data-ttu-id="74f5c-651">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-651">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="74f5c-652">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律會 `null`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-652">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="74f5c-653">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-653">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="74f5c-654">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-654">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="74f5c-655">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-655">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="74f5c-656">連接記錄</span><span class="sxs-lookup"><span data-stu-id="74f5c-656">Connection logging</span></span>

<span data-ttu-id="74f5c-657">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>，針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="74f5c-657">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="74f5c-658">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="74f5c-658">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="74f5c-659">如果 `UseConnectionLogging` 放在 `UseHttps`之前，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="74f5c-659">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="74f5c-660">如果 `UseConnectionLogging` 放在 `UseHttps`之後，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="74f5c-660">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="74f5c-661">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="74f5c-661">Bind to a TCP socket</span></span>

<span data-ttu-id="74f5c-662"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-662">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="74f5c-663">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-663">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="74f5c-664">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="74f5c-664">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="74f5c-665">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="74f5c-665">Bind to a Unix socket</span></span>

<span data-ttu-id="74f5c-666">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="74f5c-666">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="74f5c-667">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="74f5c-667">Port 0</span></span>

<span data-ttu-id="74f5c-668">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-668">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="74f5c-669">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="74f5c-669">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="74f5c-670">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="74f5c-670">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="74f5c-671">限制</span><span class="sxs-lookup"><span data-stu-id="74f5c-671">Limitations</span></span>

<span data-ttu-id="74f5c-672">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="74f5c-672">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="74f5c-673">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="74f5c-673">`--urls` command-line argument</span></span>
* <span data-ttu-id="74f5c-674">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="74f5c-674">`urls` host configuration key</span></span>
* <span data-ttu-id="74f5c-675">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="74f5c-675">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="74f5c-676">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-676">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="74f5c-677">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="74f5c-677">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="74f5c-678">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-678">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="74f5c-679">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="74f5c-679">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="74f5c-680">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="74f5c-680">IIS endpoint configuration</span></span>

<span data-ttu-id="74f5c-681">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-681">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="74f5c-682">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="74f5c-682">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="74f5c-683">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="74f5c-683">ListenOptions.Protocols</span></span>

<span data-ttu-id="74f5c-684">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-684">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="74f5c-685">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="74f5c-685">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="74f5c-686">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="74f5c-686">`HttpProtocols` enum value</span></span> | <span data-ttu-id="74f5c-687">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="74f5c-687">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="74f5c-688">僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="74f5c-688">HTTP/1.1 only.</span></span> <span data-ttu-id="74f5c-689">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-689">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="74f5c-690">僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="74f5c-690">HTTP/2 only.</span></span> <span data-ttu-id="74f5c-691">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-691">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="74f5c-692">HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="74f5c-692">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="74f5c-693">HTTP/2 需要 TLS 和[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)連線;否則，連接預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="74f5c-693">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="74f5c-694">預設通訊協定為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="74f5c-694">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="74f5c-695">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="74f5c-695">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="74f5c-696">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="74f5c-696">TLS version 1.2 or later</span></span>
* <span data-ttu-id="74f5c-697">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="74f5c-697">Renegotiation disabled</span></span>
* <span data-ttu-id="74f5c-698">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="74f5c-698">Compression disabled</span></span>
* <span data-ttu-id="74f5c-699">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="74f5c-699">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="74f5c-700">橢圓曲線 Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 最小 224 個位元</span><span class="sxs-lookup"><span data-stu-id="74f5c-700">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="74f5c-701">有限欄位 Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 最小 2048 個位元</span><span class="sxs-lookup"><span data-stu-id="74f5c-701">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="74f5c-702">加密套件未列於封鎖清單中</span><span class="sxs-lookup"><span data-stu-id="74f5c-702">Cipher suite not blacklisted</span></span>

<span data-ttu-id="74f5c-703">預設支援 `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; 與 P-256 橢圓曲線 &lbrack;`FIPS186`&rbrack;。</span><span class="sxs-lookup"><span data-stu-id="74f5c-703">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="74f5c-704">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="74f5c-704">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="74f5c-705">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="74f5c-705">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="74f5c-706">選擇性地建立 `IConnectionAdapter` 實作，針對每個連線來篩選特定加密的 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="74f5c-706">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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
        // No encryption is used with a CipherAlgorithmType.Null cipher algorithm.
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

<span data-ttu-id="74f5c-707">從設定進行通訊協定設定</span><span class="sxs-lookup"><span data-stu-id="74f5c-707">*Set the protocol from configuration*</span></span>

<span data-ttu-id="74f5c-708"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-708"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="74f5c-709">在下列 *appsettings.json* 範例中，會為 Kestrel 的所有端點建立預設連線通訊協定 (HTTP/1.1 和 HTTP/2)：</span><span class="sxs-lookup"><span data-stu-id="74f5c-709">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="74f5c-710">下列設定檔範例會為特定端點建立連線通訊協定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-710">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="74f5c-711">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="74f5c-711">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="74f5c-712">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="74f5c-712">Transport configuration</span></span>

<span data-ttu-id="74f5c-713">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="74f5c-713">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="74f5c-714">對於升級到 2.1 且會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> 並相依於下列任一套件的 ASP.NET Core 2.0 應用程式來說，這是一項中斷性變更：</span><span class="sxs-lookup"><span data-stu-id="74f5c-714">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="74f5c-715">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="74f5c-715">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="74f5c-716">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="74f5c-716">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="74f5c-717">針對需要使用 Libuv 的專案：</span><span class="sxs-lookup"><span data-stu-id="74f5c-717">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="74f5c-718">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="74f5c-718">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="74f5c-719">呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="74f5c-719">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="74f5c-720">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="74f5c-720">URL prefixes</span></span>

<span data-ttu-id="74f5c-721">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="74f5c-721">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="74f5c-722">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="74f5c-722">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="74f5c-723">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-723">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="74f5c-724">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-724">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="74f5c-725">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="74f5c-725">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="74f5c-726">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-726">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="74f5c-727">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="74f5c-727">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="74f5c-728">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-728">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="74f5c-729">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="74f5c-729">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="74f5c-730">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="74f5c-730">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="74f5c-731">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-731">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="74f5c-732">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-732">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="74f5c-733">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-733">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="74f5c-734">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="74f5c-734">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="74f5c-735">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="74f5c-735">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="74f5c-736">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="74f5c-736">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="74f5c-737">主機篩選</span><span class="sxs-lookup"><span data-stu-id="74f5c-737">Host filtering</span></span>

<span data-ttu-id="74f5c-738">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="74f5c-738">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="74f5c-739">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="74f5c-739">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="74f5c-740">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="74f5c-740">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="74f5c-741">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-741">`Host` headers aren't validated.</span></span>

<span data-ttu-id="74f5c-742">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="74f5c-742">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="74f5c-743">主機篩選中介軟體是由[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)（ASP.NET Core 2.1 或2.2）所包含的[HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件所提供。</span><span class="sxs-lookup"><span data-stu-id="74f5c-743">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="74f5c-744">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="74f5c-744">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="74f5c-745">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="74f5c-745">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="74f5c-746">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="74f5c-746">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="74f5c-747">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="74f5c-747">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="74f5c-748">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="74f5c-748">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="74f5c-749">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="74f5c-749">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="74f5c-750">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="74f5c-750">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="74f5c-751">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-751">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="74f5c-752">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-752">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="74f5c-753">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="74f5c-753">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="74f5c-754">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-754">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="74f5c-755">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="74f5c-755">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="74f5c-756">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="74f5c-756">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="74f5c-757">HTTPS</span><span class="sxs-lookup"><span data-stu-id="74f5c-757">HTTPS</span></span>
* <span data-ttu-id="74f5c-758">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="74f5c-758">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="74f5c-759">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="74f5c-759">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="74f5c-760">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-760">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="74f5c-761">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74f5c-761">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="74f5c-762">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="74f5c-762">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="74f5c-763">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-763">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="74f5c-764">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-764">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="74f5c-765">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="74f5c-765">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="74f5c-767">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="74f5c-767">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="74f5c-769">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-769">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="74f5c-770">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-770">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="74f5c-771">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="74f5c-771">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="74f5c-772">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-772">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="74f5c-773">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="74f5c-773">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="74f5c-774">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="74f5c-774">A reverse proxy:</span></span>

* <span data-ttu-id="74f5c-775">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="74f5c-775">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="74f5c-776">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="74f5c-776">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="74f5c-777">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="74f5c-777">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="74f5c-778">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="74f5c-778">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="74f5c-779">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="74f5c-779">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="74f5c-780">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-780">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="74f5c-781">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="74f5c-781">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="74f5c-782">[Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)套件包含在 AspNetCore 中。[應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-782">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="74f5c-783">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-783">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="74f5c-784">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="74f5c-784">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="74f5c-785">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="74f5c-785">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="74f5c-786">如需 `CreateDefaultBuilder` 和建立主機的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host> 的*設定主機*一節。</span><span class="sxs-lookup"><span data-stu-id="74f5c-786">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="74f5c-787">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="74f5c-787">Kestrel options</span></span>

<span data-ttu-id="74f5c-788">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-788">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="74f5c-789">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="74f5c-789">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="74f5c-790">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="74f5c-790">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="74f5c-791">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="74f5c-791">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="74f5c-792">Kestrel 選項（在C#下列範例的程式碼中設定）也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-792">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="74f5c-793">例如，檔案設定提供者可以從*appsettings*或 Appsettings 載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="74f5c-793">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="74f5c-794">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="74f5c-794">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="74f5c-795">在 `Startup.ConfigureServices`中設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="74f5c-795">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="74f5c-796">將 `IConfiguration` 的實例插入 `Startup` 類別中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-796">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="74f5c-797">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="74f5c-797">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="74f5c-798">在 `Startup.ConfigureServices` 中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-798">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="74f5c-799">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="74f5c-799">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="74f5c-800">在*Program.cs*中，將設定的 [`Kestrel`] 區段載入 Kestrel 的設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-800">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="74f5c-801">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-801">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="74f5c-802">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="74f5c-802">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="74f5c-803">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-803">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="74f5c-804">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="74f5c-804">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="74f5c-805">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-805">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="74f5c-806">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="74f5c-806">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="74f5c-807">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-807">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="74f5c-808">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-808">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="74f5c-809">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-809">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="74f5c-810">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="74f5c-810">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="74f5c-811">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="74f5c-811">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="74f5c-812">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="74f5c-812">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="74f5c-813">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="74f5c-813">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="74f5c-814">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-814">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="74f5c-815">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="74f5c-815">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="74f5c-816">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="74f5c-816">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="74f5c-817">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="74f5c-817">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="74f5c-818">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="74f5c-818">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="74f5c-819">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="74f5c-819">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="74f5c-820">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="74f5c-820">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="74f5c-821">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="74f5c-821">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="74f5c-822">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="74f5c-822">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="74f5c-823">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="74f5c-823">A minimum rate also applies to the response.</span></span> <span data-ttu-id="74f5c-824">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="74f5c-824">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="74f5c-825">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="74f5c-825">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="74f5c-826">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="74f5c-826">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="74f5c-827">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="74f5c-827">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="74f5c-828">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="74f5c-828">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="74f5c-829">同步 IO</span><span class="sxs-lookup"><span data-stu-id="74f5c-829">Synchronous IO</span></span>

<span data-ttu-id="74f5c-830"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> 控制是否允許要求與回應的同步 IO。</span><span class="sxs-lookup"><span data-stu-id="74f5c-830"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="74f5c-831">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-831">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="74f5c-832">大量的封鎖同步 IO 作業會導致執行緒集區耗盡，這會使得應用程式沒有回應。</span><span class="sxs-lookup"><span data-stu-id="74f5c-832">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="74f5c-833">只有當使用不支援同步 IO 的程式庫時才啟用 `AllowSynchronousIO`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-833">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="74f5c-834">下列範例會停用同步 IO：</span><span class="sxs-lookup"><span data-stu-id="74f5c-834">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="74f5c-835">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="74f5c-835">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="74f5c-836">端點組態</span><span class="sxs-lookup"><span data-stu-id="74f5c-836">Endpoint configuration</span></span>

<span data-ttu-id="74f5c-837">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="74f5c-837">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="74f5c-838">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="74f5c-838">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="74f5c-839">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="74f5c-839">Specify URLs using the:</span></span>

* <span data-ttu-id="74f5c-840">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="74f5c-840">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="74f5c-841">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="74f5c-841">`--urls` command-line argument.</span></span>
* <span data-ttu-id="74f5c-842">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="74f5c-842">`urls` host configuration key.</span></span>
* <span data-ttu-id="74f5c-843">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="74f5c-843">`UseUrls` extension method.</span></span>

<span data-ttu-id="74f5c-844">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-844">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="74f5c-845">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000; http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-845">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="74f5c-846">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-846">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="74f5c-847">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="74f5c-847">A development certificate is created:</span></span>

* <span data-ttu-id="74f5c-848">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="74f5c-848">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="74f5c-849">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-849">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="74f5c-850">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-850">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="74f5c-851">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-851">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="74f5c-852">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-852">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="74f5c-853">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-853">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="74f5c-854">`KestrelServerOptions` 設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-854">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="74f5c-855">ConfigureEndpointDefaults （Action\<Listenoptions 來 >）</span><span class="sxs-lookup"><span data-stu-id="74f5c-855">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="74f5c-856">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-856">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="74f5c-857">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-857">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="74f5c-858">ConfigureHttpsDefaults （Action\<HttpsConnectionAdapterOptions >）</span><span class="sxs-lookup"><span data-stu-id="74f5c-858">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="74f5c-859">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-859">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="74f5c-860">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-860">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="74f5c-861">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="74f5c-861">Configure(IConfiguration)</span></span>

<span data-ttu-id="74f5c-862">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="74f5c-862">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="74f5c-863">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="74f5c-863">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="74f5c-864">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="74f5c-864">ListenOptions.UseHttps</span></span>

<span data-ttu-id="74f5c-865">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-865">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="74f5c-866">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="74f5c-866">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="74f5c-867">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-867">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="74f5c-868">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="74f5c-868">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="74f5c-869">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="74f5c-869">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="74f5c-870">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="74f5c-870">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="74f5c-871">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="74f5c-871">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="74f5c-872">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-872">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="74f5c-873">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-873">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="74f5c-874">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="74f5c-874">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="74f5c-875">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="74f5c-875">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="74f5c-876">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-876">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="74f5c-877">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="74f5c-877">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="74f5c-878">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-878">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="74f5c-879">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-879">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="74f5c-880">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-880">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="74f5c-881">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="74f5c-881">Supported configurations described next:</span></span>

* <span data-ttu-id="74f5c-882">無組態</span><span class="sxs-lookup"><span data-stu-id="74f5c-882">No configuration</span></span>
* <span data-ttu-id="74f5c-883">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="74f5c-883">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="74f5c-884">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="74f5c-884">Change the defaults in code</span></span>

<span data-ttu-id="74f5c-885">*無組態*</span><span class="sxs-lookup"><span data-stu-id="74f5c-885">*No configuration*</span></span>

<span data-ttu-id="74f5c-886">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-886">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="74f5c-887">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="74f5c-887">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="74f5c-888">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-888">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="74f5c-889">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="74f5c-889">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="74f5c-890">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="74f5c-890">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="74f5c-891">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="74f5c-891">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="74f5c-892">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-892">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="74f5c-893">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-893">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="74f5c-894">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-894">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="74f5c-895">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="74f5c-895">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="74f5c-896">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="74f5c-896">Schema notes:</span></span>

* <span data-ttu-id="74f5c-897">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="74f5c-897">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="74f5c-898">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="74f5c-898">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="74f5c-899">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="74f5c-899">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="74f5c-900">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="74f5c-900">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="74f5c-901">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="74f5c-901">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="74f5c-902">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="74f5c-902">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="74f5c-903">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="74f5c-903">The `Certificate` section is optional.</span></span> <span data-ttu-id="74f5c-904">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="74f5c-904">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="74f5c-905">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="74f5c-905">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="74f5c-906">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-906">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="74f5c-907">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="74f5c-907">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="74f5c-908">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-908">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="74f5c-909">`KestrelServerOptions.ConfigurationLoader` 可以直接存取，以繼續逐一查看現有的載入器，例如 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所提供的載入器。</span><span class="sxs-lookup"><span data-stu-id="74f5c-909">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="74f5c-910">每個端點的 [設定] 區段都可以在 `Endpoint` 方法的選項中使用，如此一來，就可以讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-910">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="74f5c-911">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="74f5c-911">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="74f5c-912">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-912">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="74f5c-913">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="74f5c-913">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="74f5c-914">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="74f5c-914">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="74f5c-915">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="74f5c-915">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="74f5c-916">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="74f5c-916">*Change the defaults in code*</span></span>

<span data-ttu-id="74f5c-917">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-917">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="74f5c-918">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="74f5c-918">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="74f5c-919">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="74f5c-919">*Kestrel support for SNI*</span></span>

<span data-ttu-id="74f5c-920">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="74f5c-920">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="74f5c-921">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-921">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="74f5c-922">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="74f5c-922">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="74f5c-923">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="74f5c-923">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="74f5c-924">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-924">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="74f5c-925">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="74f5c-925">SNI support requires:</span></span>

* <span data-ttu-id="74f5c-926">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-926">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="74f5c-927">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律會 `null`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-927">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="74f5c-928">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-928">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="74f5c-929">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="74f5c-929">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="74f5c-930">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-930">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="74f5c-931">連接記錄</span><span class="sxs-lookup"><span data-stu-id="74f5c-931">Connection logging</span></span>

<span data-ttu-id="74f5c-932">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>，針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="74f5c-932">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="74f5c-933">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="74f5c-933">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="74f5c-934">如果 `UseConnectionLogging` 放在 `UseHttps`之前，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="74f5c-934">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="74f5c-935">如果 `UseConnectionLogging` 放在 `UseHttps`之後，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="74f5c-935">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="74f5c-936">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="74f5c-936">Bind to a TCP socket</span></span>

<span data-ttu-id="74f5c-937"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="74f5c-937">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="74f5c-938">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-938">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="74f5c-939">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="74f5c-939">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="74f5c-940">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="74f5c-940">Bind to a Unix socket</span></span>

<span data-ttu-id="74f5c-941">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="74f5c-941">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

### <a name="port-0"></a><span data-ttu-id="74f5c-942">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="74f5c-942">Port 0</span></span>

<span data-ttu-id="74f5c-943">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="74f5c-943">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="74f5c-944">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="74f5c-944">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="74f5c-945">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="74f5c-945">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="74f5c-946">限制</span><span class="sxs-lookup"><span data-stu-id="74f5c-946">Limitations</span></span>

<span data-ttu-id="74f5c-947">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="74f5c-947">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="74f5c-948">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="74f5c-948">`--urls` command-line argument</span></span>
* <span data-ttu-id="74f5c-949">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="74f5c-949">`urls` host configuration key</span></span>
* <span data-ttu-id="74f5c-950">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="74f5c-950">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="74f5c-951">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="74f5c-951">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="74f5c-952">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="74f5c-952">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="74f5c-953">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-953">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="74f5c-954">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="74f5c-954">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="74f5c-955">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="74f5c-955">IIS endpoint configuration</span></span>

<span data-ttu-id="74f5c-956">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="74f5c-956">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="74f5c-957">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="74f5c-957">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="74f5c-958">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="74f5c-958">Transport configuration</span></span>

<span data-ttu-id="74f5c-959">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="74f5c-959">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="74f5c-960">對於升級到 2.1 且會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> 並相依於下列任一套件的 ASP.NET Core 2.0 應用程式來說，這是一項中斷性變更：</span><span class="sxs-lookup"><span data-stu-id="74f5c-960">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="74f5c-961">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="74f5c-961">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="74f5c-962">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="74f5c-962">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="74f5c-963">針對需要使用 Libuv 的專案：</span><span class="sxs-lookup"><span data-stu-id="74f5c-963">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="74f5c-964">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="74f5c-964">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="74f5c-965">呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="74f5c-965">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="74f5c-966">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="74f5c-966">URL prefixes</span></span>

<span data-ttu-id="74f5c-967">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="74f5c-967">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="74f5c-968">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="74f5c-968">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="74f5c-969">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="74f5c-969">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="74f5c-970">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-970">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="74f5c-971">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="74f5c-971">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="74f5c-972">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-972">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="74f5c-973">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="74f5c-973">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="74f5c-974">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-974">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="74f5c-975">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="74f5c-975">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="74f5c-976">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="74f5c-976">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="74f5c-977">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-977">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="74f5c-978">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="74f5c-978">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="74f5c-979">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="74f5c-979">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="74f5c-980">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="74f5c-980">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="74f5c-981">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="74f5c-981">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="74f5c-982">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="74f5c-982">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="74f5c-983">主機篩選</span><span class="sxs-lookup"><span data-stu-id="74f5c-983">Host filtering</span></span>

<span data-ttu-id="74f5c-984">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="74f5c-984">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="74f5c-985">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="74f5c-985">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="74f5c-986">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="74f5c-986">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="74f5c-987">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="74f5c-987">`Host` headers aren't validated.</span></span>

<span data-ttu-id="74f5c-988">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="74f5c-988">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="74f5c-989">主機篩選中介軟體是由[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)（ASP.NET Core 2.1 或2.2）所包含的[HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件所提供。</span><span class="sxs-lookup"><span data-stu-id="74f5c-989">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="74f5c-990">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="74f5c-990">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="74f5c-991">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="74f5c-991">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="74f5c-992">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="74f5c-992">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="74f5c-993">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="74f5c-993">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="74f5c-994">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="74f5c-994">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="74f5c-995">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="74f5c-995">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="74f5c-996">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="74f5c-996">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="74f5c-997">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-997">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="74f5c-998">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="74f5c-998">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="74f5c-999">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="74f5c-999">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="74f5c-1000">其他資源</span><span class="sxs-lookup"><span data-stu-id="74f5c-1000">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="74f5c-1001">RFC 7230：訊息語法和路由 (第 5.4 節：主機)</span><span class="sxs-lookup"><span data-stu-id="74f5c-1001">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
