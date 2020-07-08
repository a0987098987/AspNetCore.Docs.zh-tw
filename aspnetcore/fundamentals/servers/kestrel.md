---
title: ASP.NET Core 中的 Kestrel 網頁伺服器實作
author: rick-anderson
description: 了解 Kestrel，這是 ASP.NET Core 的跨平台網頁伺服器。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/04/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 03c25c103f03c3f9b17311f468d96907d2498641
ms.sourcegitcommit: fa89d6553378529ae86b388689ac2c6f38281bb9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060380"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="d95b5-103">ASP.NET Core 中的 Kestrel 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="d95b5-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="d95b5-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="d95b5-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d95b5-105">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="d95b5-106">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="d95b5-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="d95b5-107">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="d95b5-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="d95b5-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="d95b5-108">HTTPS</span></span>
* <span data-ttu-id="d95b5-109">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="d95b5-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="d95b5-110">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="d95b5-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="d95b5-111">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="d95b5-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="d95b5-112">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="d95b5-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="d95b5-113">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="d95b5-114">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="d95b5-114">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="d95b5-115">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="d95b5-115">HTTP/2 support</span></span>

<span data-ttu-id="d95b5-116">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="d95b5-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="d95b5-117">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="d95b5-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="d95b5-118">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="d95b5-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="d95b5-119">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="d95b5-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="d95b5-120">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d95b5-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="d95b5-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="d95b5-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="d95b5-122">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="d95b5-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="d95b5-123">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="d95b5-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="d95b5-124">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="d95b5-125">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="d95b5-126">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="d95b5-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="d95b5-127">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="d95b5-128">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="d95b5-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="d95b5-129">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="d95b5-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="d95b5-130">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="d95b5-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="d95b5-131">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」\*\* 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="d95b5-132">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="d95b5-133">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="d95b5-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="d95b5-135">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="d95b5-137">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="d95b5-138">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="d95b5-139">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="d95b5-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="d95b5-140">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="d95b5-141">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="d95b5-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="d95b5-142">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="d95b5-142">A reverse proxy:</span></span>

* <span data-ttu-id="d95b5-143">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="d95b5-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="d95b5-144">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="d95b5-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="d95b5-145">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="d95b5-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="d95b5-146">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="d95b5-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="d95b5-147">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="d95b5-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="d95b5-148">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="kestrel-in-aspnet-core-apps"></a><span data-ttu-id="d95b5-149">ASP.NET Core 應用程式中的 Kestrel</span><span class="sxs-lookup"><span data-stu-id="d95b5-149">Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="d95b5-150">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="d95b5-151">在*Program.cs*中， <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> 方法會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> ：</span><span class="sxs-lookup"><span data-stu-id="d95b5-151">In *Program.cs*, the <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> method calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=8)]

<span data-ttu-id="d95b5-152">如需建立主機的詳細資訊，請參閱的*設定主機*和預設產生器*設定*章節 <xref:fundamentals/host/generic-host#set-up-a-host> 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-152">For more information on building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="d95b5-153">若要在呼叫 `ConfigureWebHostDefaults` 之後提供額外的設定，請使用 `ConfigureKestrel`：</span><span class="sxs-lookup"><span data-stu-id="d95b5-153">To provide additional configuration after calling `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="d95b5-154">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="d95b5-154">Kestrel options</span></span>

<span data-ttu-id="d95b5-155">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="d95b5-156">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="d95b5-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="d95b5-157">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="d95b5-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="d95b5-158">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="d95b5-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="d95b5-159">在本文稍後所示的範例中，Kestrel 選項是在 c # 程式碼中設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-159">In examples shown later in this article, Kestrel options are configured in C# code.</span></span> <span data-ttu-id="d95b5-160">您也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定 Kestrel 選項。</span><span class="sxs-lookup"><span data-stu-id="d95b5-160">Kestrel options can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="d95b5-161">例如，檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)可以從或 appsettings 的*appsettings.js*載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="d95b5-161">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider) can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="d95b5-162"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>和[端點](#endpoint-configuration)設定可從設定提供者進行設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-162"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> and [endpoint configuration](#endpoint-configuration) are configurable from configuration providers.</span></span> <span data-ttu-id="d95b5-163">其餘的 Kestrel 設定必須以 c # 程式碼進行設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-163">Remaining Kestrel configuration must be configured in C# code.</span></span>

<span data-ttu-id="d95b5-164">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="d95b5-164">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="d95b5-165">在中設定 Kestrel `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d95b5-165">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="d95b5-166">將的實例插入 `IConfiguration` 至 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="d95b5-166">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="d95b5-167">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d95b5-167">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="d95b5-168">在中 `Startup.ConfigureServices` ，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-168">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

     ```csharp
     using Microsoft.Extensions.Configuration
     
     public class Startup
     {
         public Startup(IConfiguration configuration)
         {
             Configuration = configuration;
         }

         public IConfiguration Configuration { get; }

         public void ConfigureServices(IServiceCollection services)
         {
             services.Configure<KestrelServerOptions>(
                 Configuration.GetSection("Kestrel"));
         }

         public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
         {
             ...
         }
     }
     ```

* <span data-ttu-id="d95b5-169">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="d95b5-169">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="d95b5-170">在*Program.cs*中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-170">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="d95b5-171">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-171">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="d95b5-172">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="d95b5-172">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="d95b5-173">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-173">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="d95b5-174">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="d95b5-174">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="d95b5-175">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-175">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="d95b5-176">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="d95b5-176">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="d95b5-177">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-177">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="d95b5-178">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-178">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="d95b5-179">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-179">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="d95b5-180">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-180">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="d95b5-181">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="d95b5-181">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="d95b5-182">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="d95b5-182">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="d95b5-183">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="d95b5-183">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="d95b5-184">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-184">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="d95b5-185">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="d95b5-185">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="d95b5-186">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="d95b5-186">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="d95b5-187">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-187">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="d95b5-188">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="d95b5-188">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="d95b5-189">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="d95b5-189">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="d95b5-190">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="d95b5-190">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="d95b5-191">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="d95b5-191">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="d95b5-192">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="d95b5-192">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="d95b5-193">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="d95b5-193">A minimum rate also applies to the response.</span></span> <span data-ttu-id="d95b5-194">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="d95b5-194">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="d95b5-195">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="d95b5-195">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="d95b5-196">覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="d95b5-196">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="d95b5-197">因為通訊協定對要求多工的支援，所以 HTTP/2 一般不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> 不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="d95b5-197">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="d95b5-198">不過，<xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> 仍存在 HTTP/2 要求的 `HttpContext.Features`您仍能透過將 `IHttpMinRequestBodyDataRateFeature.MinDataRate` 設定為 `null` (即使是針對 HTTP/2 要求)，以個別要求基礎來「完全停用」\*\* 讀取素率限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-198">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="d95b5-199">嘗試讀取 `IHttpMinRequestBodyDataRateFeature.MinDataRate` 或嘗試將它設定為 `null` 以外的值將會導致擲回 `NotSupportedException` (假設要求是 HTTP/2 要求)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-199">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="d95b5-200">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="d95b5-200">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="d95b5-201">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="d95b5-201">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="d95b5-202">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-202">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="d95b5-203">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="d95b5-203">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="d95b5-204">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-204">Maximum streams per connection</span></span>

<span data-ttu-id="d95b5-205">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="d95b5-205">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="d95b5-206">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="d95b5-206">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="d95b5-207">預設值是 100。</span><span class="sxs-lookup"><span data-stu-id="d95b5-207">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="d95b5-208">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="d95b5-208">Header table size</span></span>

<span data-ttu-id="d95b5-209">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="d95b5-209">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="d95b5-210">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="d95b5-210">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="d95b5-211">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-211">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="d95b5-212">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="d95b5-212">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="d95b5-213">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-213">Maximum frame size</span></span>

<span data-ttu-id="d95b5-214">`Http2.MaxFrameSize`指出伺服器所接收或傳送之 HTTP/2 連接框架承載的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-214">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="d95b5-215">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="d95b5-215">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="d95b5-216">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-216">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="d95b5-217">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-217">Maximum request header size</span></span>

<span data-ttu-id="d95b5-218">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-218">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="d95b5-219">這項限制適用于其壓縮和未壓縮標記法中的名稱和值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-219">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="d95b5-220">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-220">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="d95b5-221">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="d95b5-221">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="d95b5-222">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="d95b5-222">Initial connection window size</span></span>

<span data-ttu-id="d95b5-223">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-223">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="d95b5-224">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-224">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="d95b5-225">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-225">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="d95b5-226">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-226">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="d95b5-227">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="d95b5-227">Initial stream window size</span></span>

<span data-ttu-id="d95b5-228">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-228">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="d95b5-229">要求也皆受 `Http2.InitialConnectionWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-229">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="d95b5-230">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-230">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="d95b5-231">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-231">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="d95b5-232">同步 I/O</span><span class="sxs-lookup"><span data-stu-id="d95b5-232">Synchronous I/O</span></span>

<span data-ttu-id="d95b5-233"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>控制要求和回應是否允許同步的 i/o。</span><span class="sxs-lookup"><span data-stu-id="d95b5-233"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous I/O is allowed for the request and response.</span></span> <span data-ttu-id="d95b5-234">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-234">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="d95b5-235">大量封鎖同步 i/o 作業可能會導致執行緒集區耗盡，讓應用程式無回應。</span><span class="sxs-lookup"><span data-stu-id="d95b5-235">A large number of blocking synchronous I/O operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="d95b5-236">只有 `AllowSynchronousIO` 在使用不支援非同步 i/o 的程式庫時才啟用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-236">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous I/O.</span></span>

<span data-ttu-id="d95b5-237">下列範例會啟用同步 i/o：</span><span class="sxs-lookup"><span data-stu-id="d95b5-237">The following example enables synchronous I/O:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="d95b5-238">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="d95b5-238">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="d95b5-239">端點組態</span><span class="sxs-lookup"><span data-stu-id="d95b5-239">Endpoint configuration</span></span>

<span data-ttu-id="d95b5-240">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="d95b5-240">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="d95b5-241">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="d95b5-241">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="d95b5-242">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="d95b5-242">Specify URLs using the:</span></span>

* <span data-ttu-id="d95b5-243">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="d95b5-243">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="d95b5-244">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="d95b5-244">`--urls` command-line argument.</span></span>
* <span data-ttu-id="d95b5-245">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d95b5-245">`urls` host configuration key.</span></span>
* <span data-ttu-id="d95b5-246">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="d95b5-246">`UseUrls` extension method.</span></span>

<span data-ttu-id="d95b5-247">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-247">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="d95b5-248">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000;http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-248">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="d95b5-249">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-249">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="d95b5-250">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="d95b5-250">A development certificate is created:</span></span>

* <span data-ttu-id="d95b5-251">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="d95b5-251">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="d95b5-252">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-252">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="d95b5-253">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-253">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="d95b5-254">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-254">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="d95b5-255">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-255">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="d95b5-256">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-256">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="d95b5-257">`KestrelServerOptions`配置</span><span class="sxs-lookup"><span data-stu-id="d95b5-257">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="d95b5-258">ConfigureEndpointDefaults （動作 \<ListenOptions> ）</span><span class="sxs-lookup"><span data-stu-id="d95b5-258">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="d95b5-259">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-259">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="d95b5-260">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-260">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

> [!NOTE]
> <span data-ttu-id="d95b5-261"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-261">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="d95b5-262">ConfigureHttpsDefaults （動作 \<HttpsConnectionAdapterOptions> ）</span><span class="sxs-lookup"><span data-stu-id="d95b5-262">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="d95b5-263">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-263">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="d95b5-264">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-264">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="d95b5-265"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-265">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="d95b5-266">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="d95b5-266">Configure(IConfiguration)</span></span>

<span data-ttu-id="d95b5-267">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-267">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="d95b5-268">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="d95b5-268">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="d95b5-269">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="d95b5-269">ListenOptions.UseHttps</span></span>

<span data-ttu-id="d95b5-270">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-270">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="d95b5-271">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="d95b5-271">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="d95b5-272">`UseHttps`：將 Kestrel 設定為使用 HTTPS 搭配預設憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-272">`UseHttps`: Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="d95b5-273">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d95b5-273">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="d95b5-274">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="d95b5-274">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="d95b5-275">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="d95b5-275">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="d95b5-276">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="d95b5-276">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="d95b5-277">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-277">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="d95b5-278">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-278">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="d95b5-279">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="d95b5-279">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="d95b5-280">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="d95b5-280">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="d95b5-281">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-281">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="d95b5-282">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="d95b5-282">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="d95b5-283">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-283">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="d95b5-284">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-284">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="d95b5-285">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-285">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="d95b5-286">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="d95b5-286">Supported configurations described next:</span></span>

* <span data-ttu-id="d95b5-287">無組態</span><span class="sxs-lookup"><span data-stu-id="d95b5-287">No configuration</span></span>
* <span data-ttu-id="d95b5-288">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="d95b5-288">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="d95b5-289">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="d95b5-289">Change the defaults in code</span></span>

<span data-ttu-id="d95b5-290">*無組態*</span><span class="sxs-lookup"><span data-stu-id="d95b5-290">*No configuration*</span></span>

<span data-ttu-id="d95b5-291">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-291">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="d95b5-292">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="d95b5-292">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="d95b5-293">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-293">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="d95b5-294">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="d95b5-294">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="d95b5-295">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="d95b5-295">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="d95b5-296">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-296">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="d95b5-297">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-297">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="d95b5-298">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證]**[預設]** > \*\*\*\* 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-298">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="d95b5-299">除了針對任何憑證節點使用 [路徑]\*\*\*\* 和 [密碼]\*\*\*\*，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-299">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="d95b5-300">例如，**憑證**  >  **預設**憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="d95b5-300">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="d95b5-301">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="d95b5-301">Schema notes:</span></span>

* <span data-ttu-id="d95b5-302">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d95b5-302">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="d95b5-303">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="d95b5-303">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="d95b5-304">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="d95b5-304">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="d95b5-305">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-305">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="d95b5-306">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="d95b5-306">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="d95b5-307">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="d95b5-307">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="d95b5-308">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="d95b5-308">The `Certificate` section is optional.</span></span> <span data-ttu-id="d95b5-309">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-309">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="d95b5-310">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="d95b5-310">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="d95b5-311">`Certificate`一節同時支援**路徑** &ndash; **密碼**和**主體** &ndash; **存放區**憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-311">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="d95b5-312">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="d95b5-312">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="d95b5-313">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-313">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="d95b5-314">`KestrelServerOptions.ConfigurationLoader`可以直接存取，以繼續逐一查看現有的載入器，例如所提供的載入器 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-314">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="d95b5-315">您可以在方法的選項中取得每個端點的設定區段， `Endpoint` 以便讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-315">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="d95b5-316">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="d95b5-316">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="d95b5-317">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-317">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="d95b5-318">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="d95b5-318">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="d95b5-319">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="d95b5-319">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="d95b5-320">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="d95b5-320">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="d95b5-321">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="d95b5-321">*Change the defaults in code*</span></span>

<span data-ttu-id="d95b5-322">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-322">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="d95b5-323">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="d95b5-323">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="d95b5-324">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="d95b5-324">*Kestrel support for SNI*</span></span>

<span data-ttu-id="d95b5-325">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="d95b5-325">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="d95b5-326">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-326">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="d95b5-327">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="d95b5-327">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="d95b5-328">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="d95b5-328">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="d95b5-329">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-329">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="d95b5-330">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="d95b5-330">SNI support requires:</span></span>

* <span data-ttu-id="d95b5-331">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-331">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="d95b5-332">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律為 `null` 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-332">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="d95b5-333">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-333">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="d95b5-334">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-334">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="d95b5-335">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-335">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="d95b5-336">連接記錄</span><span class="sxs-lookup"><span data-stu-id="d95b5-336">Connection logging</span></span>

<span data-ttu-id="d95b5-337">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> ，以針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="d95b5-337">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="d95b5-338">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="d95b5-338">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="d95b5-339">如果 `UseConnectionLogging` 放在之前 `UseHttps` ，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="d95b5-339">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="d95b5-340">如果 `UseConnectionLogging` 放在之後 `UseHttps` ，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="d95b5-340">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="d95b5-341">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="d95b5-341">Bind to a TCP socket</span></span>

<span data-ttu-id="d95b5-342"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-342">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=12-18)]

<span data-ttu-id="d95b5-343">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-343">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="d95b5-344">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="d95b5-344">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="d95b5-345">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="d95b5-345">Bind to a Unix socket</span></span>

<span data-ttu-id="d95b5-346">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="d95b5-346">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

* <span data-ttu-id="d95b5-347">在 Nginx 設定檔中，將 `server`  >  `location`  >  `proxy_pass` 專案設定為 `http://unix:/tmp/{KESTREL SOCKET}:/;` 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-347">In the Nginx configuration file, set the `server` > `location` > `proxy_pass` entry to `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span></span> <span data-ttu-id="d95b5-348">`{KESTREL SOCKET}`這是提供給的通訊端名稱 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> （例如， `kestrel-test.sock` 在上述範例中為）。</span><span class="sxs-lookup"><span data-stu-id="d95b5-348">`{KESTREL SOCKET}` is the name of the socket provided to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (for example, `kestrel-test.sock` in the preceding example).</span></span>
* <span data-ttu-id="d95b5-349">請確定通訊端可由 Nginx 寫入（例如， `chmod go+w /tmp/kestrel-test.sock` ）。</span><span class="sxs-lookup"><span data-stu-id="d95b5-349">Ensure that the socket is writeable by Nginx (for example, `chmod go+w /tmp/kestrel-test.sock`).</span></span>

### <a name="port-0"></a><span data-ttu-id="d95b5-350">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="d95b5-350">Port 0</span></span>

<span data-ttu-id="d95b5-351">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-351">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="d95b5-352">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="d95b5-352">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="d95b5-353">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="d95b5-353">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="d95b5-354">限制</span><span class="sxs-lookup"><span data-stu-id="d95b5-354">Limitations</span></span>

<span data-ttu-id="d95b5-355">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="d95b5-355">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="d95b5-356">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="d95b5-356">`--urls` command-line argument</span></span>
* <span data-ttu-id="d95b5-357">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="d95b5-357">`urls` host configuration key</span></span>
* <span data-ttu-id="d95b5-358">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="d95b5-358">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="d95b5-359">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-359">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="d95b5-360">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="d95b5-360">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="d95b5-361">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-361">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="d95b5-362">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="d95b5-362">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="d95b5-363">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="d95b5-363">IIS endpoint configuration</span></span>

<span data-ttu-id="d95b5-364">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-364">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="d95b5-365">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="d95b5-365">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="d95b5-366">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="d95b5-366">ListenOptions.Protocols</span></span>

<span data-ttu-id="d95b5-367">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-367">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="d95b5-368">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d95b5-368">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="d95b5-369">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="d95b5-369">`HttpProtocols` enum value</span></span> | <span data-ttu-id="d95b5-370">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="d95b5-370">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="d95b5-371">僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d95b5-371">HTTP/1.1 only.</span></span> <span data-ttu-id="d95b5-372">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-372">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="d95b5-373">僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="d95b5-373">HTTP/2 only.</span></span> <span data-ttu-id="d95b5-374">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-374">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="d95b5-375">HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="d95b5-375">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="d95b5-376">HTTP/2 要求用戶端選取 TLS[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)交握中的 HTTP/2;否則，連接預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d95b5-376">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="d95b5-377">`ListenOptions.Protocols`任何端點的預設值為 `HttpProtocols.Http1AndHttp2` 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-377">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="d95b5-378">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="d95b5-378">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="d95b5-379">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="d95b5-379">TLS version 1.2 or later</span></span>
* <span data-ttu-id="d95b5-380">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="d95b5-380">Renegotiation disabled</span></span>
* <span data-ttu-id="d95b5-381">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="d95b5-381">Compression disabled</span></span>
* <span data-ttu-id="d95b5-382">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="d95b5-382">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="d95b5-383">橢圓曲線 Diffie-hellman （ECDHE） &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; ：224位最小值</span><span class="sxs-lookup"><span data-stu-id="d95b5-383">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;: 224 bits minimum</span></span>
  * <span data-ttu-id="d95b5-384">有限欄位 diffie-hellman （DHE） &lbrack; `TLS12` &rbrack; ：2048位最小值</span><span class="sxs-lookup"><span data-stu-id="d95b5-384">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;: 2048 bits minimum</span></span>
* <span data-ttu-id="d95b5-385">不禁止加密套件。</span><span class="sxs-lookup"><span data-stu-id="d95b5-385">Cipher suite not prohibited.</span></span> 

<span data-ttu-id="d95b5-386">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`&lbrack;`TLS-ECDHE`&rbrack;根據預設，支援 P-256 橢圓曲線 &lbrack; `FIPS186` &rbrack; 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-386">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="d95b5-387">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="d95b5-387">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="d95b5-388">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="d95b5-388">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="d95b5-389">如有需要，請使用連線中介軟體，針對特定的加密依據每個連線來篩選 TLS 交握。</span><span class="sxs-lookup"><span data-stu-id="d95b5-389">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="d95b5-390">下列範例 <xref:System.NotSupportedException> 會針對應用程式不支援的任何加密演算法擲回。</span><span class="sxs-lookup"><span data-stu-id="d95b5-390">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="d95b5-391">或者，定義 ITlsHandshakeFeature，並將[CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm)與可接受的加密套件清單進行比較。</span><span class="sxs-lookup"><span data-stu-id="d95b5-391">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="d95b5-392">不會使用加密來搭配[CipherAlgorithmType。 Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher 演算法。</span><span class="sxs-lookup"><span data-stu-id="d95b5-392">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

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

<span data-ttu-id="d95b5-393">連接篩選也可以透過 <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda 設定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-393">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

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

<span data-ttu-id="d95b5-394">在 Linux 上， <xref:System.Net.Security.CipherSuitesPolicy> 可以用來篩選每個連線的 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="d95b5-394">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

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

<span data-ttu-id="d95b5-395">從設定進行通訊協定設定\*\*</span><span class="sxs-lookup"><span data-stu-id="d95b5-395">*Set the protocol from configuration*</span></span>

<span data-ttu-id="d95b5-396">`CreateDefaultBuilder` 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-396">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="d95b5-397">下列*appsettings.js*範例會將 HTTP/1.1 建立為所有端點的預設連接通訊協定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-397">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="d95b5-398">下列*appsettings.js*範例會建立特定端點的 HTTP/1.1 連接通訊協定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-398">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="d95b5-399">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-399">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="d95b5-400">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="d95b5-400">Transport configuration</span></span>

<span data-ttu-id="d95b5-401">針對需要使用 Libuv （）的專案 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> ：</span><span class="sxs-lookup"><span data-stu-id="d95b5-401">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="d95b5-402">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-402">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="d95b5-403"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>在上呼叫 `IWebHostBuilder` ：</span><span class="sxs-lookup"><span data-stu-id="d95b5-403">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="d95b5-404">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="d95b5-404">URL prefixes</span></span>

<span data-ttu-id="d95b5-405">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="d95b5-405">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="d95b5-406">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="d95b5-406">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="d95b5-407">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-407">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="d95b5-408">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-408">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="d95b5-409">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="d95b5-409">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="d95b5-410">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-410">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="d95b5-411">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="d95b5-411">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="d95b5-412">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-412">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="d95b5-413">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="d95b5-413">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="d95b5-414">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="d95b5-414">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="d95b5-415">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-415">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="d95b5-416">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-416">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="d95b5-417">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-417">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="d95b5-418">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="d95b5-418">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="d95b5-419">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="d95b5-419">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="d95b5-420">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="d95b5-420">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="d95b5-421">主機篩選</span><span class="sxs-lookup"><span data-stu-id="d95b5-421">Host filtering</span></span>

<span data-ttu-id="d95b5-422">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="d95b5-422">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="d95b5-423">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="d95b5-423">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="d95b5-424">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="d95b5-424">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="d95b5-425">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-425">`Host` headers aren't validated.</span></span>

<span data-ttu-id="d95b5-426">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d95b5-426">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="d95b5-427">主機篩選中介軟體是由[AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件提供，這是針對 ASP.NET Core 應用程式而隱含提供的。</span><span class="sxs-lookup"><span data-stu-id="d95b5-427">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="d95b5-428">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="d95b5-428">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="d95b5-429">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d95b5-429">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="d95b5-430">若要啟用中介軟體，請 `AllowedHosts` 在 appsettings*上的appsettings.js*中定義索引鍵 / \* \<EnvironmentName> 。json\*。</span><span class="sxs-lookup"><span data-stu-id="d95b5-430">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="d95b5-431">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="d95b5-431">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="d95b5-432">*appsettings.js*：</span><span class="sxs-lookup"><span data-stu-id="d95b5-432">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="d95b5-433">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="d95b5-433">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="d95b5-434">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="d95b5-434">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="d95b5-435">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-435">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="d95b5-436">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-436">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="d95b5-437">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="d95b5-437">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="d95b5-438">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-438">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="d95b5-439">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="d95b5-439">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="d95b5-440">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="d95b5-440">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="d95b5-441">HTTPS</span><span class="sxs-lookup"><span data-stu-id="d95b5-441">HTTPS</span></span>
* <span data-ttu-id="d95b5-442">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="d95b5-442">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="d95b5-443">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="d95b5-443">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="d95b5-444">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="d95b5-444">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="d95b5-445">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="d95b5-445">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="d95b5-446">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-446">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="d95b5-447">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="d95b5-447">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="d95b5-448">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="d95b5-448">HTTP/2 support</span></span>

<span data-ttu-id="d95b5-449">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="d95b5-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="d95b5-450">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="d95b5-450">Operating system&dagger;</span></span>
  * <span data-ttu-id="d95b5-451">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="d95b5-451">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="d95b5-452">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="d95b5-452">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="d95b5-453">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d95b5-453">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="d95b5-454">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="d95b5-454">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="d95b5-455">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="d95b5-455">TLS 1.2 or later connection</span></span>

<span data-ttu-id="d95b5-456">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="d95b5-456">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="d95b5-457">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-457">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="d95b5-458">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-458">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="d95b5-459">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="d95b5-459">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="d95b5-460">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-460">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="d95b5-461">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="d95b5-461">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="d95b5-462">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="d95b5-462">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="d95b5-463">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="d95b5-463">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="d95b5-464">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」\*\* 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-464">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="d95b5-465">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-465">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="d95b5-466">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="d95b5-466">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="d95b5-468">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-468">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="d95b5-470">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-470">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="d95b5-471">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-471">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="d95b5-472">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="d95b5-472">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="d95b5-473">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-473">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="d95b5-474">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="d95b5-474">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="d95b5-475">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="d95b5-475">A reverse proxy:</span></span>

* <span data-ttu-id="d95b5-476">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="d95b5-476">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="d95b5-477">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="d95b5-477">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="d95b5-478">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="d95b5-478">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="d95b5-479">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="d95b5-479">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="d95b5-480">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="d95b5-480">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="d95b5-481">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-481">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="d95b5-482">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="d95b5-482">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="d95b5-483">[Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)套件包含在 AspNetCore 中。[應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="d95b5-483">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d95b5-484">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-484">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="d95b5-485">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="d95b5-485">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="d95b5-486">如需有關 `CreateDefaultBuilder` 和建立主機的詳細資訊，請參閱的*設定主機*一節 <xref:fundamentals/host/web-host#set-up-a-host> 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-486">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="d95b5-487">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請使用 `ConfigureKestrel`：</span><span class="sxs-lookup"><span data-stu-id="d95b5-487">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="d95b5-488">如果應用程式未呼叫 `CreateDefaultBuilder` 來設定主機，請在呼叫 `ConfigureKestrel` **之前**，先呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="d95b5-488">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="d95b5-489">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="d95b5-489">Kestrel options</span></span>

<span data-ttu-id="d95b5-490">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-490">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="d95b5-491">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="d95b5-491">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="d95b5-492">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="d95b5-492">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="d95b5-493">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="d95b5-493">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="d95b5-494">在下列範例中，以 c # 程式碼設定的 Kestrel 選項也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-494">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="d95b5-495">例如，檔案設定提供者可以從或 appsettings 的*appsettings.js*載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="d95b5-495">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="d95b5-496">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="d95b5-496">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="d95b5-497">在中設定 Kestrel `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d95b5-497">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="d95b5-498">將的實例插入 `IConfiguration` 至 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="d95b5-498">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="d95b5-499">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d95b5-499">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="d95b5-500">在中 `Startup.ConfigureServices` ，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-500">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

     ```csharp
     using Microsoft.Extensions.Configuration
     
     public class Startup
     {
         public Startup(IConfiguration configuration)
         {
             Configuration = configuration;
         }

         public IConfiguration Configuration { get; }

         public void ConfigureServices(IServiceCollection services)
         {
             services.Configure<KestrelServerOptions>(
                 Configuration.GetSection("Kestrel"));
         }

         public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
         {
             ...
         }
     }
     ```

* <span data-ttu-id="d95b5-501">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="d95b5-501">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="d95b5-502">在*Program.cs*中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-502">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="d95b5-503">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-503">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="d95b5-504">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="d95b5-504">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="d95b5-505">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-505">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="d95b5-506">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="d95b5-506">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="d95b5-507">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-507">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="d95b5-508">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="d95b5-508">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="d95b5-509">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-509">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="d95b5-510">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-510">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="d95b5-511">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-511">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="d95b5-512">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-512">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="d95b5-513">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="d95b5-513">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="d95b5-514">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="d95b5-514">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="d95b5-515">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="d95b5-515">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="d95b5-516">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-516">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="d95b5-517">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="d95b5-517">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="d95b5-518">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="d95b5-518">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="d95b5-519">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-519">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="d95b5-520">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="d95b5-520">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="d95b5-521">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="d95b5-521">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="d95b5-522">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="d95b5-522">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="d95b5-523">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="d95b5-523">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="d95b5-524">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="d95b5-524">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="d95b5-525">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="d95b5-525">A minimum rate also applies to the response.</span></span> <span data-ttu-id="d95b5-526">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="d95b5-526">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="d95b5-527">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="d95b5-527">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="d95b5-528">覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="d95b5-528">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="d95b5-529">因為通訊協定對要求多工的支援，所以 HTTP/2 不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的所有速率功能都不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="d95b5-529">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="d95b5-530">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="d95b5-530">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="d95b5-531">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="d95b5-531">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="d95b5-532">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-532">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="d95b5-533">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="d95b5-533">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="d95b5-534">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-534">Maximum streams per connection</span></span>

<span data-ttu-id="d95b5-535">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="d95b5-535">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="d95b5-536">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="d95b5-536">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="d95b5-537">預設值是 100。</span><span class="sxs-lookup"><span data-stu-id="d95b5-537">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="d95b5-538">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="d95b5-538">Header table size</span></span>

<span data-ttu-id="d95b5-539">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="d95b5-539">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="d95b5-540">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="d95b5-540">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="d95b5-541">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-541">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="d95b5-542">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="d95b5-542">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="d95b5-543">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-543">Maximum frame size</span></span>

<span data-ttu-id="d95b5-544">`Http2.MaxFrameSize` 表示要接收的 HTTP/2 連線框架承載大小上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-544">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="d95b5-545">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="d95b5-545">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="d95b5-546">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-546">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="d95b5-547">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-547">Maximum request header size</span></span>

<span data-ttu-id="d95b5-548">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-548">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="d95b5-549">此限制皆共同套用至已壓縮及未壓縮代表中的名稱與值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-549">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="d95b5-550">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-550">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="d95b5-551">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="d95b5-551">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="d95b5-552">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="d95b5-552">Initial connection window size</span></span>

<span data-ttu-id="d95b5-553">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-553">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="d95b5-554">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-554">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="d95b5-555">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-555">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="d95b5-556">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-556">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="d95b5-557">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="d95b5-557">Initial stream window size</span></span>

<span data-ttu-id="d95b5-558">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-558">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="d95b5-559">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-559">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="d95b5-560">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-560">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="d95b5-561">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-561">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="d95b5-562">同步 I/O</span><span class="sxs-lookup"><span data-stu-id="d95b5-562">Synchronous I/O</span></span>

<span data-ttu-id="d95b5-563"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>控制要求和回應是否允許同步的 i/o。</span><span class="sxs-lookup"><span data-stu-id="d95b5-563"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous I/O is allowed for the request and response.</span></span> <span data-ttu-id="d95b5-564">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-564">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="d95b5-565">大量封鎖同步 i/o 作業可能會導致執行緒集區耗盡，讓應用程式無回應。</span><span class="sxs-lookup"><span data-stu-id="d95b5-565">A large number of blocking synchronous I/O operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="d95b5-566">只有 `AllowSynchronousIO` 在使用不支援非同步 i/o 的程式庫時才啟用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-566">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous I/O.</span></span>

<span data-ttu-id="d95b5-567">下列範例會啟用同步 i/o：</span><span class="sxs-lookup"><span data-stu-id="d95b5-567">The following example enables synchronous I/O:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="d95b5-568">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="d95b5-568">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="d95b5-569">端點組態</span><span class="sxs-lookup"><span data-stu-id="d95b5-569">Endpoint configuration</span></span>

<span data-ttu-id="d95b5-570">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="d95b5-570">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="d95b5-571">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="d95b5-571">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="d95b5-572">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="d95b5-572">Specify URLs using the:</span></span>

* <span data-ttu-id="d95b5-573">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="d95b5-573">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="d95b5-574">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="d95b5-574">`--urls` command-line argument.</span></span>
* <span data-ttu-id="d95b5-575">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d95b5-575">`urls` host configuration key.</span></span>
* <span data-ttu-id="d95b5-576">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="d95b5-576">`UseUrls` extension method.</span></span>

<span data-ttu-id="d95b5-577">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-577">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="d95b5-578">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000;http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-578">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="d95b5-579">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-579">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="d95b5-580">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="d95b5-580">A development certificate is created:</span></span>

* <span data-ttu-id="d95b5-581">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="d95b5-581">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="d95b5-582">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-582">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="d95b5-583">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-583">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="d95b5-584">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-584">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="d95b5-585">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-585">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="d95b5-586">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-586">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="d95b5-587">`KestrelServerOptions`配置</span><span class="sxs-lookup"><span data-stu-id="d95b5-587">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="d95b5-588">ConfigureEndpointDefaults （動作 \<ListenOptions> ）</span><span class="sxs-lookup"><span data-stu-id="d95b5-588">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="d95b5-589">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-589">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="d95b5-590">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-590">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="d95b5-591"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-591">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="d95b5-592">ConfigureHttpsDefaults （動作 \<HttpsConnectionAdapterOptions> ）</span><span class="sxs-lookup"><span data-stu-id="d95b5-592">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="d95b5-593">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-593">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="d95b5-594">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-594">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="d95b5-595"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-595">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>


### <a name="configureiconfiguration"></a><span data-ttu-id="d95b5-596">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="d95b5-596">Configure(IConfiguration)</span></span>

<span data-ttu-id="d95b5-597">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-597">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="d95b5-598">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="d95b5-598">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="d95b5-599">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="d95b5-599">ListenOptions.UseHttps</span></span>

<span data-ttu-id="d95b5-600">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-600">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="d95b5-601">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="d95b5-601">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="d95b5-602">`UseHttps`：將 Kestrel 設定為使用 HTTPS 搭配預設憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-602">`UseHttps`: Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="d95b5-603">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d95b5-603">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="d95b5-604">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="d95b5-604">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="d95b5-605">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="d95b5-605">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="d95b5-606">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="d95b5-606">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="d95b5-607">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-607">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="d95b5-608">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-608">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="d95b5-609">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="d95b5-609">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="d95b5-610">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="d95b5-610">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="d95b5-611">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-611">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="d95b5-612">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="d95b5-612">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="d95b5-613">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-613">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="d95b5-614">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-614">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="d95b5-615">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-615">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="d95b5-616">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="d95b5-616">Supported configurations described next:</span></span>

* <span data-ttu-id="d95b5-617">無組態</span><span class="sxs-lookup"><span data-stu-id="d95b5-617">No configuration</span></span>
* <span data-ttu-id="d95b5-618">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="d95b5-618">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="d95b5-619">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="d95b5-619">Change the defaults in code</span></span>

<span data-ttu-id="d95b5-620">*無組態*</span><span class="sxs-lookup"><span data-stu-id="d95b5-620">*No configuration*</span></span>

<span data-ttu-id="d95b5-621">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-621">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="d95b5-622">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="d95b5-622">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="d95b5-623">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-623">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="d95b5-624">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="d95b5-624">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="d95b5-625">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="d95b5-625">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="d95b5-626">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-626">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="d95b5-627">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-627">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="d95b5-628">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證]**[預設]** > \*\*\*\* 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-628">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="d95b5-629">除了針對任何憑證節點使用 [路徑]\*\*\*\* 和 [密碼]\*\*\*\*，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-629">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="d95b5-630">例如，**憑證**  >  **預設**憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="d95b5-630">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="d95b5-631">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="d95b5-631">Schema notes:</span></span>

* <span data-ttu-id="d95b5-632">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d95b5-632">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="d95b5-633">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="d95b5-633">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="d95b5-634">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="d95b5-634">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="d95b5-635">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-635">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="d95b5-636">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="d95b5-636">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="d95b5-637">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="d95b5-637">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="d95b5-638">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="d95b5-638">The `Certificate` section is optional.</span></span> <span data-ttu-id="d95b5-639">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-639">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="d95b5-640">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="d95b5-640">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="d95b5-641">`Certificate`一節同時支援**路徑** &ndash; **密碼**和**主體** &ndash; **存放區**憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-641">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="d95b5-642">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="d95b5-642">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="d95b5-643">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-643">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="d95b5-644">`KestrelServerOptions.ConfigurationLoader`可以直接存取，以繼續逐一查看現有的載入器，例如所提供的載入器 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-644">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="d95b5-645">您可以在方法的選項中取得每個端點的設定區段， `Endpoint` 以便讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-645">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="d95b5-646">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="d95b5-646">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="d95b5-647">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-647">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="d95b5-648">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="d95b5-648">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="d95b5-649">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="d95b5-649">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="d95b5-650">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="d95b5-650">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="d95b5-651">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="d95b5-651">*Change the defaults in code*</span></span>

<span data-ttu-id="d95b5-652">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-652">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="d95b5-653">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="d95b5-653">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="d95b5-654">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="d95b5-654">*Kestrel support for SNI*</span></span>

<span data-ttu-id="d95b5-655">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="d95b5-655">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="d95b5-656">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-656">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="d95b5-657">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="d95b5-657">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="d95b5-658">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="d95b5-658">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="d95b5-659">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-659">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="d95b5-660">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="d95b5-660">SNI support requires:</span></span>

* <span data-ttu-id="d95b5-661">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-661">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="d95b5-662">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律為 `null` 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-662">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="d95b5-663">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-663">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="d95b5-664">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-664">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="d95b5-665">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-665">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="d95b5-666">連接記錄</span><span class="sxs-lookup"><span data-stu-id="d95b5-666">Connection logging</span></span>

<span data-ttu-id="d95b5-667">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> ，以針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="d95b5-667">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="d95b5-668">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="d95b5-668">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="d95b5-669">如果 `UseConnectionLogging` 放在之前 `UseHttps` ，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="d95b5-669">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="d95b5-670">如果 `UseConnectionLogging` 放在之後 `UseHttps` ，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="d95b5-670">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="d95b5-671">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="d95b5-671">Bind to a TCP socket</span></span>

<span data-ttu-id="d95b5-672"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-672">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="d95b5-673">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-673">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="d95b5-674">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="d95b5-674">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="d95b5-675">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="d95b5-675">Bind to a Unix socket</span></span>

<span data-ttu-id="d95b5-676">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="d95b5-676">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

* <span data-ttu-id="d95b5-677">在 Nginx confiuguration 檔中，將 `server`  >  `location`  >  `proxy_pass` 專案設定為 `http://unix:/tmp/{KESTREL SOCKET}:/;` 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-677">In the Nginx confiuguration file, set the `server` > `location` > `proxy_pass` entry to `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span></span> <span data-ttu-id="d95b5-678">`{KESTREL SOCKET}`這是提供給的通訊端名稱 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> （例如， `kestrel-test.sock` 在上述範例中為）。</span><span class="sxs-lookup"><span data-stu-id="d95b5-678">`{KESTREL SOCKET}` is the name of the socket provided to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (for example, `kestrel-test.sock` in the preceding example).</span></span>
* <span data-ttu-id="d95b5-679">請確定通訊端可由 Nginx 寫入（例如， `chmod go+w /tmp/kestrel-test.sock` ）。</span><span class="sxs-lookup"><span data-stu-id="d95b5-679">Ensure that the socket is writeable by Nginx (for example, `chmod go+w /tmp/kestrel-test.sock`).</span></span> 

### <a name="port-0"></a><span data-ttu-id="d95b5-680">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="d95b5-680">Port 0</span></span>

<span data-ttu-id="d95b5-681">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-681">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="d95b5-682">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="d95b5-682">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="d95b5-683">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="d95b5-683">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="d95b5-684">限制</span><span class="sxs-lookup"><span data-stu-id="d95b5-684">Limitations</span></span>

<span data-ttu-id="d95b5-685">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="d95b5-685">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="d95b5-686">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="d95b5-686">`--urls` command-line argument</span></span>
* <span data-ttu-id="d95b5-687">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="d95b5-687">`urls` host configuration key</span></span>
* <span data-ttu-id="d95b5-688">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="d95b5-688">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="d95b5-689">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-689">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="d95b5-690">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="d95b5-690">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="d95b5-691">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-691">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="d95b5-692">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="d95b5-692">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="d95b5-693">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="d95b5-693">IIS endpoint configuration</span></span>

<span data-ttu-id="d95b5-694">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-694">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="d95b5-695">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="d95b5-695">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="d95b5-696">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="d95b5-696">ListenOptions.Protocols</span></span>

<span data-ttu-id="d95b5-697">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-697">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="d95b5-698">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d95b5-698">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="d95b5-699">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="d95b5-699">`HttpProtocols` enum value</span></span> | <span data-ttu-id="d95b5-700">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="d95b5-700">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="d95b5-701">僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d95b5-701">HTTP/1.1 only.</span></span> <span data-ttu-id="d95b5-702">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-702">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="d95b5-703">僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="d95b5-703">HTTP/2 only.</span></span> <span data-ttu-id="d95b5-704">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-704">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="d95b5-705">HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="d95b5-705">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="d95b5-706">HTTP/2 需要 TLS 和[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)連線;否則，連接預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d95b5-706">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="d95b5-707">預設通訊協定為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d95b5-707">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="d95b5-708">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="d95b5-708">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="d95b5-709">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="d95b5-709">TLS version 1.2 or later</span></span>
* <span data-ttu-id="d95b5-710">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="d95b5-710">Renegotiation disabled</span></span>
* <span data-ttu-id="d95b5-711">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="d95b5-711">Compression disabled</span></span>
* <span data-ttu-id="d95b5-712">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="d95b5-712">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="d95b5-713">橢圓曲線 Diffie-hellman （ECDHE） &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; ：224位最小值</span><span class="sxs-lookup"><span data-stu-id="d95b5-713">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;: 224 bits minimum</span></span>
  * <span data-ttu-id="d95b5-714">有限欄位 diffie-hellman （DHE） &lbrack; `TLS12` &rbrack; ：2048位最小值</span><span class="sxs-lookup"><span data-stu-id="d95b5-714">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;: 2048 bits minimum</span></span>
* <span data-ttu-id="d95b5-715">未封鎖加密套件</span><span class="sxs-lookup"><span data-stu-id="d95b5-715">Cipher suite not blocked</span></span>

<span data-ttu-id="d95b5-716">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`&lbrack;`TLS-ECDHE`&rbrack;根據預設，支援 P-256 橢圓曲線 &lbrack; `FIPS186` &rbrack; 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-716">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="d95b5-717">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="d95b5-717">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="d95b5-718">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="d95b5-718">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="d95b5-719">選擇性地建立 `IConnectionAdapter` 實作，針對每個連線來篩選特定加密的 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="d95b5-719">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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

<span data-ttu-id="d95b5-720">從設定進行通訊協定設定\*\*</span><span class="sxs-lookup"><span data-stu-id="d95b5-720">*Set the protocol from configuration*</span></span>

<span data-ttu-id="d95b5-721"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-721"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="d95b5-722">在下列 *appsettings.json* 範例中，會為 Kestrel 的所有端點建立預設連線通訊協定 (HTTP/1.1 和 HTTP/2)：</span><span class="sxs-lookup"><span data-stu-id="d95b5-722">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="d95b5-723">下列設定檔範例會為特定端點建立連線通訊協定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-723">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="d95b5-724">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-724">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="d95b5-725">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="d95b5-725">Transport configuration</span></span>

<span data-ttu-id="d95b5-726">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="d95b5-726">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="d95b5-727">對於升級到 2.1 且會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> 並相依於下列任一套件的 ASP.NET Core 2.0 應用程式來說，這是一項中斷性變更：</span><span class="sxs-lookup"><span data-stu-id="d95b5-727">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="d95b5-728">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="d95b5-728">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="d95b5-729">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="d95b5-729">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="d95b5-730">針對需要使用 Libuv 的專案：</span><span class="sxs-lookup"><span data-stu-id="d95b5-730">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="d95b5-731">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-731">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="d95b5-732">呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="d95b5-732">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="d95b5-733">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="d95b5-733">URL prefixes</span></span>

<span data-ttu-id="d95b5-734">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="d95b5-734">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="d95b5-735">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="d95b5-735">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="d95b5-736">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-736">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="d95b5-737">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-737">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="d95b5-738">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="d95b5-738">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="d95b5-739">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-739">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="d95b5-740">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="d95b5-740">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="d95b5-741">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-741">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="d95b5-742">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="d95b5-742">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="d95b5-743">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="d95b5-743">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="d95b5-744">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-744">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="d95b5-745">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-745">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="d95b5-746">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-746">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="d95b5-747">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="d95b5-747">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="d95b5-748">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="d95b5-748">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="d95b5-749">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="d95b5-749">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="d95b5-750">主機篩選</span><span class="sxs-lookup"><span data-stu-id="d95b5-750">Host filtering</span></span>

<span data-ttu-id="d95b5-751">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="d95b5-751">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="d95b5-752">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="d95b5-752">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="d95b5-753">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="d95b5-753">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="d95b5-754">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-754">`Host` headers aren't validated.</span></span>

<span data-ttu-id="d95b5-755">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d95b5-755">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="d95b5-756">主機篩選中介軟體是由[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)（ASP.NET Core 2.1 或2.2）所包含的[HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件所提供。</span><span class="sxs-lookup"><span data-stu-id="d95b5-756">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="d95b5-757">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="d95b5-757">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="d95b5-758">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d95b5-758">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="d95b5-759">若要啟用中介軟體，請 `AllowedHosts` 在 appsettings*上的appsettings.js*中定義索引鍵 / \* \<EnvironmentName> 。json\*。</span><span class="sxs-lookup"><span data-stu-id="d95b5-759">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="d95b5-760">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="d95b5-760">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="d95b5-761">*appsettings.js*：</span><span class="sxs-lookup"><span data-stu-id="d95b5-761">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="d95b5-762">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="d95b5-762">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="d95b5-763">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="d95b5-763">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="d95b5-764">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-764">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="d95b5-765">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-765">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="d95b5-766">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="d95b5-766">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="d95b5-767">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-767">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="d95b5-768">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="d95b5-768">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="d95b5-769">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="d95b5-769">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="d95b5-770">HTTPS</span><span class="sxs-lookup"><span data-stu-id="d95b5-770">HTTPS</span></span>
* <span data-ttu-id="d95b5-771">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="d95b5-771">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="d95b5-772">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="d95b5-772">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="d95b5-773">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-773">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="d95b5-774">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="d95b5-774">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="d95b5-775">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="d95b5-775">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="d95b5-776">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」\*\* 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-776">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="d95b5-777">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-777">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="d95b5-778">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="d95b5-778">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="d95b5-780">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-780">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="d95b5-782">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-782">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="d95b5-783">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-783">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="d95b5-784">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="d95b5-784">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="d95b5-785">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-785">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="d95b5-786">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="d95b5-786">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="d95b5-787">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="d95b5-787">A reverse proxy:</span></span>

* <span data-ttu-id="d95b5-788">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="d95b5-788">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="d95b5-789">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="d95b5-789">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="d95b5-790">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="d95b5-790">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="d95b5-791">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="d95b5-791">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="d95b5-792">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="d95b5-792">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="d95b5-793">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-793">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="d95b5-794">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="d95b5-794">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="d95b5-795">[Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)套件包含在 AspNetCore 中。[應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="d95b5-795">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d95b5-796">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-796">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="d95b5-797">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="d95b5-797">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="d95b5-798">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="d95b5-798">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="d95b5-799">如需有關 `CreateDefaultBuilder` 和建立主機的詳細資訊，請參閱的*設定主機*一節 <xref:fundamentals/host/web-host#set-up-a-host> 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-799">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="d95b5-800">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="d95b5-800">Kestrel options</span></span>

<span data-ttu-id="d95b5-801">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-801">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="d95b5-802">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="d95b5-802">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="d95b5-803">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="d95b5-803">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="d95b5-804">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="d95b5-804">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="d95b5-805">在下列範例中，以 c # 程式碼設定的 Kestrel 選項也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-805">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="d95b5-806">例如，檔案設定提供者可以從或 appsettings 的*appsettings.js*載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="d95b5-806">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="d95b5-807">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="d95b5-807">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="d95b5-808">在中設定 Kestrel `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d95b5-808">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="d95b5-809">將的實例插入 `IConfiguration` 至 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="d95b5-809">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="d95b5-810">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d95b5-810">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="d95b5-811">在中 `Startup.ConfigureServices` ，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-811">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

     ```csharp
     using Microsoft.Extensions.Configuration
     
     public class Startup
     {
         public Startup(IConfiguration configuration)
         {
             Configuration = configuration;
         }

         public IConfiguration Configuration { get; }

         public void ConfigureServices(IServiceCollection services)
         {
             services.Configure<KestrelServerOptions>(
                 Configuration.GetSection("Kestrel"));
         }

         public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
         {
             ...
         }
     }
     ```

* <span data-ttu-id="d95b5-812">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="d95b5-812">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="d95b5-813">在*Program.cs*中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-813">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="d95b5-814">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-814">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="d95b5-815">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="d95b5-815">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="d95b5-816">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-816">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="d95b5-817">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="d95b5-817">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="d95b5-818">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-818">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="d95b5-819">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="d95b5-819">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="d95b5-820">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-820">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="d95b5-821">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-821">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="d95b5-822">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-822">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="d95b5-823">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="d95b5-823">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="d95b5-824">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="d95b5-824">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="d95b5-825">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="d95b5-825">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="d95b5-826">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="d95b5-826">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="d95b5-827">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-827">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="d95b5-828">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="d95b5-828">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="d95b5-829">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="d95b5-829">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="d95b5-830">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="d95b5-830">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="d95b5-831">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="d95b5-831">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="d95b5-832">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="d95b5-832">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="d95b5-833">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="d95b5-833">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="d95b5-834">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="d95b5-834">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="d95b5-835">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="d95b5-835">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="d95b5-836">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="d95b5-836">A minimum rate also applies to the response.</span></span> <span data-ttu-id="d95b5-837">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="d95b5-837">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="d95b5-838">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="d95b5-838">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="d95b5-839">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="d95b5-839">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="d95b5-840">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="d95b5-840">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="d95b5-841">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="d95b5-841">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="d95b5-842">同步 I/O</span><span class="sxs-lookup"><span data-stu-id="d95b5-842">Synchronous I/O</span></span>

<span data-ttu-id="d95b5-843"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>控制要求和回應是否允許同步的 i/o。</span><span class="sxs-lookup"><span data-stu-id="d95b5-843"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous I/O is allowed for the request and response.</span></span> <span data-ttu-id="d95b5-844">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-844">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="d95b5-845">大量封鎖同步 i/o 作業可能會導致執行緒集區耗盡，讓應用程式無回應。</span><span class="sxs-lookup"><span data-stu-id="d95b5-845">A large number of blocking synchronous I/O operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="d95b5-846">只有 `AllowSynchronousIO` 在使用不支援非同步 i/o 的程式庫時才啟用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-846">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous I/O.</span></span>

<span data-ttu-id="d95b5-847">下列範例會停用同步 i/o：</span><span class="sxs-lookup"><span data-stu-id="d95b5-847">The following example disables synchronous I/O:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="d95b5-848">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="d95b5-848">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="d95b5-849">端點組態</span><span class="sxs-lookup"><span data-stu-id="d95b5-849">Endpoint configuration</span></span>

<span data-ttu-id="d95b5-850">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="d95b5-850">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="d95b5-851">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="d95b5-851">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="d95b5-852">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="d95b5-852">Specify URLs using the:</span></span>

* <span data-ttu-id="d95b5-853">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="d95b5-853">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="d95b5-854">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="d95b5-854">`--urls` command-line argument.</span></span>
* <span data-ttu-id="d95b5-855">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d95b5-855">`urls` host configuration key.</span></span>
* <span data-ttu-id="d95b5-856">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="d95b5-856">`UseUrls` extension method.</span></span>

<span data-ttu-id="d95b5-857">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-857">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="d95b5-858">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000;http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-858">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="d95b5-859">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-859">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="d95b5-860">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="d95b5-860">A development certificate is created:</span></span>

* <span data-ttu-id="d95b5-861">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="d95b5-861">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="d95b5-862">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-862">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="d95b5-863">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-863">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="d95b5-864">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-864">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="d95b5-865">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-865">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="d95b5-866">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-866">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="d95b5-867">`KestrelServerOptions`配置</span><span class="sxs-lookup"><span data-stu-id="d95b5-867">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="d95b5-868">ConfigureEndpointDefaults （動作 \<ListenOptions> ）</span><span class="sxs-lookup"><span data-stu-id="d95b5-868">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="d95b5-869">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-869">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="d95b5-870">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-870">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="d95b5-871"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-871">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="d95b5-872">ConfigureHttpsDefaults （動作 \<HttpsConnectionAdapterOptions> ）</span><span class="sxs-lookup"><span data-stu-id="d95b5-872">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="d95b5-873">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-873">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="d95b5-874">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-874">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="d95b5-875"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-875">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="d95b5-876">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="d95b5-876">Configure(IConfiguration)</span></span>

<span data-ttu-id="d95b5-877">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d95b5-877">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="d95b5-878">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="d95b5-878">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="d95b5-879">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="d95b5-879">ListenOptions.UseHttps</span></span>

<span data-ttu-id="d95b5-880">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-880">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="d95b5-881">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="d95b5-881">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="d95b5-882">`UseHttps`：將 Kestrel 設定為使用 HTTPS 搭配預設憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-882">`UseHttps`: Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="d95b5-883">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d95b5-883">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="d95b5-884">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="d95b5-884">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="d95b5-885">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="d95b5-885">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="d95b5-886">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="d95b5-886">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="d95b5-887">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-887">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="d95b5-888">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-888">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="d95b5-889">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="d95b5-889">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="d95b5-890">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="d95b5-890">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="d95b5-891">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-891">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="d95b5-892">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="d95b5-892">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="d95b5-893">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-893">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="d95b5-894">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-894">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="d95b5-895">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-895">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="d95b5-896">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="d95b5-896">Supported configurations described next:</span></span>

* <span data-ttu-id="d95b5-897">無組態</span><span class="sxs-lookup"><span data-stu-id="d95b5-897">No configuration</span></span>
* <span data-ttu-id="d95b5-898">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="d95b5-898">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="d95b5-899">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="d95b5-899">Change the defaults in code</span></span>

<span data-ttu-id="d95b5-900">*無組態*</span><span class="sxs-lookup"><span data-stu-id="d95b5-900">*No configuration*</span></span>

<span data-ttu-id="d95b5-901">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-901">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="d95b5-902">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="d95b5-902">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="d95b5-903">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-903">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="d95b5-904">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="d95b5-904">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="d95b5-905">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="d95b5-905">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="d95b5-906">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-906">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="d95b5-907">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-907">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="d95b5-908">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證]**[預設]** > \*\*\*\* 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-908">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="d95b5-909">除了針對任何憑證節點使用 [路徑]\*\*\*\* 和 [密碼]\*\*\*\*，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-909">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="d95b5-910">例如，**憑證**  >  **預設**憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="d95b5-910">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="d95b5-911">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="d95b5-911">Schema notes:</span></span>

* <span data-ttu-id="d95b5-912">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d95b5-912">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="d95b5-913">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="d95b5-913">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="d95b5-914">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="d95b5-914">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="d95b5-915">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-915">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="d95b5-916">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="d95b5-916">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="d95b5-917">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="d95b5-917">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="d95b5-918">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="d95b5-918">The `Certificate` section is optional.</span></span> <span data-ttu-id="d95b5-919">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="d95b5-919">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="d95b5-920">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="d95b5-920">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="d95b5-921">`Certificate`一節同時支援**路徑** &ndash; **密碼**和**主體** &ndash; **存放區**憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-921">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="d95b5-922">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="d95b5-922">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="d95b5-923">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-923">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="d95b5-924">`KestrelServerOptions.ConfigurationLoader`可以直接存取，以繼續逐一查看現有的載入器，例如所提供的載入器 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-924">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="d95b5-925">您可以在方法的選項中取得每個端點的設定區段， `Endpoint` 以便讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-925">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="d95b5-926">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="d95b5-926">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="d95b5-927">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-927">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="d95b5-928">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="d95b5-928">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="d95b5-929">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="d95b5-929">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="d95b5-930">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="d95b5-930">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="d95b5-931">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="d95b5-931">*Change the defaults in code*</span></span>

<span data-ttu-id="d95b5-932">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-932">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="d95b5-933">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="d95b5-933">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="d95b5-934">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="d95b5-934">*Kestrel support for SNI*</span></span>

<span data-ttu-id="d95b5-935">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="d95b5-935">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="d95b5-936">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-936">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="d95b5-937">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="d95b5-937">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="d95b5-938">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="d95b5-938">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="d95b5-939">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-939">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="d95b5-940">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="d95b5-940">SNI support requires:</span></span>

* <span data-ttu-id="d95b5-941">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-941">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="d95b5-942">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律為 `null` 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-942">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="d95b5-943">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-943">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="d95b5-944">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="d95b5-944">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="d95b5-945">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-945">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="d95b5-946">連接記錄</span><span class="sxs-lookup"><span data-stu-id="d95b5-946">Connection logging</span></span>

<span data-ttu-id="d95b5-947">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> ，以針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="d95b5-947">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="d95b5-948">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="d95b5-948">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="d95b5-949">如果 `UseConnectionLogging` 放在之前 `UseHttps` ，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="d95b5-949">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="d95b5-950">如果 `UseConnectionLogging` 放在之後 `UseHttps` ，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="d95b5-950">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="d95b5-951">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="d95b5-951">Bind to a TCP socket</span></span>

<span data-ttu-id="d95b5-952"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="d95b5-952">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="d95b5-953">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-953">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="d95b5-954">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="d95b5-954">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="d95b5-955">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="d95b5-955">Bind to a Unix socket</span></span>

<span data-ttu-id="d95b5-956">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="d95b5-956">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

* <span data-ttu-id="d95b5-957">在 Nginx confiuguration 檔中，將 `server`  >  `location`  >  `proxy_pass` 專案設定為 `http://unix:/tmp/{KESTREL SOCKET}:/;` 。</span><span class="sxs-lookup"><span data-stu-id="d95b5-957">In the Nginx confiuguration file, set the `server` > `location` > `proxy_pass` entry to `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span></span> <span data-ttu-id="d95b5-958">`{KESTREL SOCKET}`這是提供給的通訊端名稱 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> （例如， `kestrel-test.sock` 在上述範例中為）。</span><span class="sxs-lookup"><span data-stu-id="d95b5-958">`{KESTREL SOCKET}` is the name of the socket provided to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (for example, `kestrel-test.sock` in the preceding example).</span></span>
* <span data-ttu-id="d95b5-959">請確定通訊端可由 Nginx 寫入（例如， `chmod go+w /tmp/kestrel-test.sock` ）。</span><span class="sxs-lookup"><span data-stu-id="d95b5-959">Ensure that the socket is writeable by Nginx (for example, `chmod go+w /tmp/kestrel-test.sock`).</span></span> 

### <a name="port-0"></a><span data-ttu-id="d95b5-960">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="d95b5-960">Port 0</span></span>

<span data-ttu-id="d95b5-961">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="d95b5-961">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="d95b5-962">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="d95b5-962">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="d95b5-963">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="d95b5-963">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="d95b5-964">限制</span><span class="sxs-lookup"><span data-stu-id="d95b5-964">Limitations</span></span>

<span data-ttu-id="d95b5-965">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="d95b5-965">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="d95b5-966">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="d95b5-966">`--urls` command-line argument</span></span>
* <span data-ttu-id="d95b5-967">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="d95b5-967">`urls` host configuration key</span></span>
* <span data-ttu-id="d95b5-968">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="d95b5-968">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="d95b5-969">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-969">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="d95b5-970">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="d95b5-970">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="d95b5-971">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-971">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="d95b5-972">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="d95b5-972">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="d95b5-973">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="d95b5-973">IIS endpoint configuration</span></span>

<span data-ttu-id="d95b5-974">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-974">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="d95b5-975">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="d95b5-975">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="d95b5-976">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="d95b5-976">Transport configuration</span></span>

<span data-ttu-id="d95b5-977">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="d95b5-977">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="d95b5-978">對於升級到 2.1 且會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> 並相依於下列任一套件的 ASP.NET Core 2.0 應用程式來說，這是一項中斷性變更：</span><span class="sxs-lookup"><span data-stu-id="d95b5-978">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="d95b5-979">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="d95b5-979">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="d95b5-980">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="d95b5-980">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="d95b5-981">針對需要使用 Libuv 的專案：</span><span class="sxs-lookup"><span data-stu-id="d95b5-981">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="d95b5-982">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-982">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="d95b5-983">呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="d95b5-983">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="d95b5-984">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="d95b5-984">URL prefixes</span></span>

<span data-ttu-id="d95b5-985">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="d95b5-985">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="d95b5-986">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="d95b5-986">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="d95b5-987">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d95b5-987">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="d95b5-988">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-988">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="d95b5-989">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="d95b5-989">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="d95b5-990">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-990">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="d95b5-991">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="d95b5-991">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="d95b5-992">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-992">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="d95b5-993">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="d95b5-993">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="d95b5-994">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="d95b5-994">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="d95b5-995">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-995">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="d95b5-996">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-996">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="d95b5-997">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="d95b5-997">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="d95b5-998">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="d95b5-998">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="d95b5-999">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="d95b5-999">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="d95b5-1000">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1000">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="d95b5-1001">主機篩選</span><span class="sxs-lookup"><span data-stu-id="d95b5-1001">Host filtering</span></span>

<span data-ttu-id="d95b5-1002">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1002">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="d95b5-1003">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1003">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="d95b5-1004">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1004">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="d95b5-1005">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1005">`Host` headers aren't validated.</span></span>

<span data-ttu-id="d95b5-1006">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1006">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="d95b5-1007">主機篩選中介軟體是由[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)（ASP.NET Core 2.1 或2.2）所包含的[HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件所提供。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1007">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="d95b5-1008">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="d95b5-1008">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="d95b5-1009">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1009">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="d95b5-1010">若要啟用中介軟體，請 `AllowedHosts` 在 appsettings*上的appsettings.js*中定義索引鍵 / \* \<EnvironmentName> 。json\*。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1010">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="d95b5-1011">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="d95b5-1011">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="d95b5-1012">*appsettings.js*：</span><span class="sxs-lookup"><span data-stu-id="d95b5-1012">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="d95b5-1013">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1013">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="d95b5-1014">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1014">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="d95b5-1015">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1015">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="d95b5-1016">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1016">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="d95b5-1017">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1017">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="http11-request-draining"></a><span data-ttu-id="d95b5-1018">HTTP/1.1 要求清空</span><span class="sxs-lookup"><span data-stu-id="d95b5-1018">HTTP/1.1 request draining</span></span>

<span data-ttu-id="d95b5-1019">開啟 HTTP 連接非常耗時。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1019">Opening HTTP connections is time consuming.</span></span> <span data-ttu-id="d95b5-1020">針對 HTTPS，這也是耗用大量資源。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1020">For HTTPS, it's also resource intensive.</span></span> <span data-ttu-id="d95b5-1021">因此，Kestrel 會嘗試每個 HTTP/1.1 通訊協定重複使用連接。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1021">Therefore, Kestrel tries to reuse connections per the HTTP/1.1 protocol.</span></span> <span data-ttu-id="d95b5-1022">要求主體必須完全取用，才能重複使用連接。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1022">A request body must be fully consumed to allow the connection to be reused.</span></span> <span data-ttu-id="d95b5-1023">應用程式不一定會使用要求主體，例如伺服器傳回重新 `POST` 導向或404回應的要求。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1023">The app doesn't always consume the request body, such as a `POST` requests where the server returns a redirect or 404 response.</span></span> <span data-ttu-id="d95b5-1024">在 `POST` -重新導向案例中：</span><span class="sxs-lookup"><span data-stu-id="d95b5-1024">In the `POST`-redirect case:</span></span>

* <span data-ttu-id="d95b5-1025">用戶端可能已經傳送部分 `POST` 資料。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1025">The client may already have sent part of the `POST` data.</span></span>
* <span data-ttu-id="d95b5-1026">伺服器會寫入301回應。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1026">The server writes the 301 response.</span></span>
* <span data-ttu-id="d95b5-1027">連接無法用於新的要求，直到 `POST` 先前要求主體的資料完全讀取為止。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1027">The connection can't be used for a new request until the `POST` data from the previous request body has been fully read.</span></span>
* <span data-ttu-id="d95b5-1028">Kestrel 嘗試清空要求主體。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1028">Kestrel tries to drain the request body.</span></span> <span data-ttu-id="d95b5-1029">清空要求本文表示讀取和捨棄資料，而不進行處理。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1029">Draining the request body means reading and discarding the data without processing it.</span></span>

<span data-ttu-id="d95b5-1030">清空程式可讓您在允許重複使用連線時，以及釋放任何剩餘資料所需的時間之間進行 tradoff：</span><span class="sxs-lookup"><span data-stu-id="d95b5-1030">The draining process makes a tradoff between allowing the connection to be reused and the time it takes to drain any remaining data:</span></span>

* <span data-ttu-id="d95b5-1031">清空的超時時間為5秒，無法設定。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1031">Draining has a timeout of five seconds, which isn't configurable.</span></span>
* <span data-ttu-id="d95b5-1032">如果或標頭所指定的所有資料都 `Content-Length` `Transfer-Encoding` 未在超時之前讀取，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1032">If all of the data specified by the `Content-Length` or `Transfer-Encoding` header hasn't been read before the timeout, the connection is closed.</span></span>

<span data-ttu-id="d95b5-1033">有時候，您可能會想要在寫入回應之前或之後立即終止要求。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1033">Sometimes you may want to terminate the request immediately, before or after writing the response.</span></span> <span data-ttu-id="d95b5-1034">例如，用戶端可能會有限制的資料上限，因此限制上傳的資料可能是優先順序。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1034">For example, clients may have restrictive data caps, so limiting uploaded data might be a priority.</span></span> <span data-ttu-id="d95b5-1035">在這種情況下，若要終止要求，請從控制器、頁面或中介軟體中呼叫[HttpCoNtext. Abort。](xref:Microsoft.AspNetCore.Http.HttpContext.Abort%2A) Razor</span><span class="sxs-lookup"><span data-stu-id="d95b5-1035">In such cases to terminate a request, call [HttpContext.Abort](xref:Microsoft.AspNetCore.Http.HttpContext.Abort%2A) from a controller, Razor Page, or middleware.</span></span>

<span data-ttu-id="d95b5-1036">呼叫有幾點注意事項 `Abort` ：</span><span class="sxs-lookup"><span data-stu-id="d95b5-1036">There are caveats to calling `Abort`:</span></span>

* <span data-ttu-id="d95b5-1037">建立新的連接可能會變慢且昂貴。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1037">Creating new connections can be slow and expensive.</span></span>
* <span data-ttu-id="d95b5-1038">不保證用戶端在連接關閉之前已讀取回應。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1038">There's no guarantee that the client has read the response before the connection closes.</span></span>
* <span data-ttu-id="d95b5-1039">呼叫 `Abort` 應該很罕見，並保留給嚴重的錯誤案例，而不是常見的錯誤。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1039">Calling `Abort` should be rare and reserved for severe error cases, not common errors.</span></span>
  * <span data-ttu-id="d95b5-1040">只有 `Abort` 在需要解決特定問題時，才呼叫。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1040">Only call `Abort` when a specific problem needs to be solved.</span></span> <span data-ttu-id="d95b5-1041">例如， `Abort` 如果惡意用戶端正在嘗試資料， `POST` 或當用戶端程式代碼中發生錯誤而導致大型或多個要求時，請呼叫。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1041">For example, call `Abort` if malicious clients are trying to `POST` data or when there's a bug in client code that causes large or numerous requests.</span></span>
  * <span data-ttu-id="d95b5-1042">請勿呼叫 `Abort` 常見的錯誤狀況，例如 HTTP 404 （找不到）。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1042">Don't call `Abort` for common error situations, such as HTTP 404 (Not Found).</span></span>

<span data-ttu-id="d95b5-1043">呼叫[HttpResponse](xref:Microsoft.AspNetCore.Http.HttpResponse.CompleteAsync%2A)之前，請先呼叫 CompleteAsync `Abort` ，以確保伺服器已完成寫入回應。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1043">Calling [HttpResponse.CompleteAsync](xref:Microsoft.AspNetCore.Http.HttpResponse.CompleteAsync%2A) before calling `Abort` ensures that the server has completed writing the response.</span></span> <span data-ttu-id="d95b5-1044">不過，用戶端行為無法預測，而且在中斷連線之前，它們可能不會讀取回應。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1044">However, client behavior isn't predictable and they may not read the response before the connection is aborted.</span></span>

<span data-ttu-id="d95b5-1045">此程式與 HTTP/2 不同，因為通訊協定支援在不關閉連線的情況下中止個別要求資料流程。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1045">This process is different for HTTP/2 because the protocol supports aborting individual request streams without closing the connection.</span></span> <span data-ttu-id="d95b5-1046">五秒的清空超時不適用。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1046">The five second drain timeout doesn't apply.</span></span> <span data-ttu-id="d95b5-1047">如果在完成回應之後有任何未讀取的要求本文資料，伺服器就會傳送 HTTP/2 RST 框架。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1047">If there's any unread request body data after completing a response, then the server sends an HTTP/2 RST frame.</span></span> <span data-ttu-id="d95b5-1048">系統會忽略其他要求主體資料框架。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1048">Additional request body data frames are ignored.</span></span>

<span data-ttu-id="d95b5-1049">可能的話，用戶端最好先利用[預期的： 100-continue](https://developer.mozilla.org/docs/Web/HTTP/Status/100)要求標頭，等待伺服器回應，然後再開始傳送要求本文。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1049">If possible, it's better for clients to utilize the [Expect: 100-continue](https://developer.mozilla.org/docs/Web/HTTP/Status/100) request header and wait for the server to respond before starting to send the request body.</span></span> <span data-ttu-id="d95b5-1050">這讓用戶端有機會在傳送不需要的資料之前，先檢查回應並中止。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1050">That gives the client an opportunity to examine the response and abort before sending unneeded data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d95b5-1051">其他資源</span><span class="sxs-lookup"><span data-stu-id="d95b5-1051">Additional resources</span></span>

* <span data-ttu-id="d95b5-1052">在 Linux 上使用 UNIX 通訊端時，通訊端不會在應用程式關閉時自動刪除。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1052">When using UNIX sockets on Linux, the socket is not automatically deleted on app shut down.</span></span> <span data-ttu-id="d95b5-1053">如需詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/aspnetcore/issues/14134)。</span><span class="sxs-lookup"><span data-stu-id="d95b5-1053">For more information, see [this GitHub issue](https://github.com/dotnet/aspnetcore/issues/14134).</span></span>
* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="d95b5-1054">RFC 7230：訊息語法和路由 (第 5.4 節：主機)</span><span class="sxs-lookup"><span data-stu-id="d95b5-1054">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
