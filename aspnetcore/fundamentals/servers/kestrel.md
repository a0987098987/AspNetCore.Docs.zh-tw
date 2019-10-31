---
title: ASP.NET Core 中的 Kestrel 網頁伺服器實作
author: guardrex
description: 了解 Kestrel，這是 ASP.NET Core 的跨平台網頁伺服器。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/29/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: beaf6ac49359adfdc2dc24221eab04cc853646a9
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143438"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="27bd8-103">ASP.NET Core 中的 Kestrel 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="27bd8-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="27bd8-104">由[Tom 作者: dykstra](https://github.com/tdykstra)、 [Chris Ross](https://github.com/Tratcher)、 [Stephen Halter](https://twitter.com/halter73)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="27bd8-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="27bd8-105">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="27bd8-106">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="27bd8-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="27bd8-107">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="27bd8-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="27bd8-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="27bd8-108">HTTPS</span></span>
* <span data-ttu-id="27bd8-109">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="27bd8-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="27bd8-110">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="27bd8-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="27bd8-111">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="27bd8-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="27bd8-112">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="27bd8-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="27bd8-113">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="27bd8-114">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="27bd8-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="27bd8-115">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="27bd8-115">HTTP/2 support</span></span>

<span data-ttu-id="27bd8-116">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="27bd8-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="27bd8-117">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="27bd8-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="27bd8-118">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="27bd8-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="27bd8-119">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="27bd8-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="27bd8-120">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="27bd8-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="27bd8-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="27bd8-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="27bd8-122">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="27bd8-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="27bd8-123">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="27bd8-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="27bd8-124">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="27bd8-125">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="27bd8-126">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="27bd8-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="27bd8-127">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="27bd8-128">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="27bd8-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="27bd8-129">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="27bd8-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="27bd8-130">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="27bd8-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="27bd8-131">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="27bd8-132">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="27bd8-133">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="27bd8-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="27bd8-135">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="27bd8-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="27bd8-137">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="27bd8-138">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="27bd8-139">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="27bd8-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="27bd8-140">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="27bd8-141">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="27bd8-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="27bd8-142">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="27bd8-142">A reverse proxy:</span></span>

* <span data-ttu-id="27bd8-143">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="27bd8-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="27bd8-144">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="27bd8-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="27bd8-145">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="27bd8-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="27bd8-146">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="27bd8-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="27bd8-147">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="27bd8-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="27bd8-148">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="27bd8-149">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="27bd8-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="27bd8-150">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="27bd8-151">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="27bd8-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="27bd8-152">如需 `CreateDefaultBuilder` 和建立主機的詳細資訊，請參閱 <xref:fundamentals/host/generic-host#set-up-a-host> 的*設定主機*和預設產生器*設定*章節。</span><span class="sxs-lookup"><span data-stu-id="27bd8-152">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="27bd8-153">若要在呼叫 `CreateDefaultBuilder` 並 `ConfigureWebHostDefaults` 之後提供額外的設定，請使用 `ConfigureKestrel`：</span><span class="sxs-lookup"><span data-stu-id="27bd8-153">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

<span data-ttu-id="27bd8-154">如果應用程式未呼叫 `CreateDefaultBuilder` 來設定主機，請在呼叫 `ConfigureKestrel` **之前**，先呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="27bd8-154">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="27bd8-155">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="27bd8-155">Kestrel options</span></span>

<span data-ttu-id="27bd8-156">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-156">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="27bd8-157">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="27bd8-157">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="27bd8-158">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="27bd8-158">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="27bd8-159">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="27bd8-159">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="27bd8-160">Kestrel 選項（在C#下列範例的程式碼中設定）也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-160">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="27bd8-161">例如，檔案設定提供者可以從*appsettings*或 Appsettings 載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="27bd8-161">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="27bd8-162">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="27bd8-162">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="27bd8-163">在 `Startup.ConfigureServices`中設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="27bd8-163">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="27bd8-164">將 `IConfiguration` 的實例插入 `Startup` 類別中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-164">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="27bd8-165">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="27bd8-165">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="27bd8-166">在 `Startup.ConfigureServices` 中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-166">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="27bd8-167">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="27bd8-167">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="27bd8-168">在*Program.cs*中，將設定的 [`Kestrel`] 區段載入 Kestrel 的設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-168">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="27bd8-169">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-169">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="27bd8-170">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="27bd8-170">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="27bd8-171">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-171">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="27bd8-172">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="27bd8-172">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="27bd8-173">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-173">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="27bd8-174">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="27bd8-174">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="27bd8-175">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-175">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="27bd8-176">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-176">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="27bd8-177">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-177">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="27bd8-178">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-178">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="27bd8-179">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="27bd8-179">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="27bd8-180">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="27bd8-180">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="27bd8-181">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="27bd8-181">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="27bd8-182">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-182">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="27bd8-183">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="27bd8-183">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="27bd8-184">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="27bd8-184">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="27bd8-185">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-185">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="27bd8-186">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="27bd8-186">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="27bd8-187">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="27bd8-187">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="27bd8-188">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="27bd8-188">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="27bd8-189">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="27bd8-189">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="27bd8-190">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="27bd8-190">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="27bd8-191">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="27bd8-191">A minimum rate also applies to the response.</span></span> <span data-ttu-id="27bd8-192">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="27bd8-192">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="27bd8-193">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="27bd8-193">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="27bd8-194">覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="27bd8-194">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="27bd8-195">因為通訊協定對要求多工的支援，所以 HTTP/2 一般不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> 不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-195">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="27bd8-196">不過，<xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> 仍存在 HTTP/2 要求的 `HttpContext.Features`您仍能透過將 `IHttpMinRequestBodyDataRateFeature.MinDataRate` 設定為 `null` (即使是針對 HTTP/2 要求)，以個別要求基礎來「完全停用」讀取素率限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-196">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="27bd8-197">嘗試讀取 `IHttpMinRequestBodyDataRateFeature.MinDataRate` 或嘗試將它設定為 `null` 以外的值將會導致擲回 `NotSupportedException` (假設要求是 HTTP/2 要求)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-197">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="27bd8-198">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="27bd8-198">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="27bd8-199">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="27bd8-199">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="27bd8-200">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-200">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="27bd8-201">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="27bd8-201">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="27bd8-202">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-202">Maximum streams per connection</span></span>

<span data-ttu-id="27bd8-203">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="27bd8-203">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="27bd8-204">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="27bd8-204">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="27bd8-205">預設值為 100。</span><span class="sxs-lookup"><span data-stu-id="27bd8-205">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="27bd8-206">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="27bd8-206">Header table size</span></span>

<span data-ttu-id="27bd8-207">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="27bd8-207">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="27bd8-208">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="27bd8-208">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="27bd8-209">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-209">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="27bd8-210">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="27bd8-210">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="27bd8-211">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-211">Maximum frame size</span></span>

<span data-ttu-id="27bd8-212">`Http2.MaxFrameSize` 表示伺服器所接收或傳送之 HTTP/2 連接框架承載的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-212">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="27bd8-213">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="27bd8-213">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="27bd8-214">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-214">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="27bd8-215">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-215">Maximum request header size</span></span>

<span data-ttu-id="27bd8-216">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-216">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="27bd8-217">這項限制適用于其壓縮和未壓縮標記法中的名稱和值。</span><span class="sxs-lookup"><span data-stu-id="27bd8-217">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="27bd8-218">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-218">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="27bd8-219">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="27bd8-219">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="27bd8-220">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="27bd8-220">Initial connection window size</span></span>

<span data-ttu-id="27bd8-221">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-221">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="27bd8-222">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-222">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="27bd8-223">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-223">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="27bd8-224">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-224">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="27bd8-225">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="27bd8-225">Initial stream window size</span></span>

<span data-ttu-id="27bd8-226">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-226">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="27bd8-227">要求也皆受 `Http2.InitialConnectionWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-227">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="27bd8-228">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-228">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="27bd8-229">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-229">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="27bd8-230">同步 IO</span><span class="sxs-lookup"><span data-stu-id="27bd8-230">Synchronous IO</span></span>

<span data-ttu-id="27bd8-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> 控制是否允許要求與回應的同步 IO。</span><span class="sxs-lookup"><span data-stu-id="27bd8-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="27bd8-232">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-232">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="27bd8-233">大量的封鎖同步 IO 作業會導致執行緒集區耗盡，這會使得應用程式沒有回應。</span><span class="sxs-lookup"><span data-stu-id="27bd8-233">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="27bd8-234">只有當使用不支援同步 IO 的程式庫時才啟用 `AllowSynchronousIO`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-234">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="27bd8-235">下列範例會啟用同步 IO：</span><span class="sxs-lookup"><span data-stu-id="27bd8-235">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="27bd8-236">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="27bd8-236">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="27bd8-237">端點組態</span><span class="sxs-lookup"><span data-stu-id="27bd8-237">Endpoint configuration</span></span>

<span data-ttu-id="27bd8-238">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="27bd8-238">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="27bd8-239">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="27bd8-239">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="27bd8-240">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="27bd8-240">Specify URLs using the:</span></span>

* <span data-ttu-id="27bd8-241">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="27bd8-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="27bd8-242">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="27bd8-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="27bd8-243">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="27bd8-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="27bd8-244">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="27bd8-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="27bd8-245">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-245">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="27bd8-246">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000; http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-246">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="27bd8-247">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-247">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="27bd8-248">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="27bd8-248">A development certificate is created:</span></span>

* <span data-ttu-id="27bd8-249">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="27bd8-249">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="27bd8-250">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-250">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="27bd8-251">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-251">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="27bd8-252">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-252">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="27bd8-253">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-253">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="27bd8-254">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-254">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="27bd8-255">`KestrelServerOptions` 設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-255">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="27bd8-256">ConfigureEndpointDefaults （Action\<Listenoptions 來 >）</span><span class="sxs-lookup"><span data-stu-id="27bd8-256">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="27bd8-257">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-257">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="27bd8-258">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-258">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="27bd8-259">ConfigureHttpsDefaults （Action\<HttpsConnectionAdapterOptions >）</span><span class="sxs-lookup"><span data-stu-id="27bd8-259">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="27bd8-260">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-260">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="27bd8-261">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-261">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="27bd8-262">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="27bd8-262">Configure(IConfiguration)</span></span>

<span data-ttu-id="27bd8-263">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-263">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="27bd8-264">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="27bd8-264">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="27bd8-265">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="27bd8-265">ListenOptions.UseHttps</span></span>

<span data-ttu-id="27bd8-266">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-266">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="27bd8-267">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="27bd8-267">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="27bd8-268">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-268">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="27bd8-269">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="27bd8-269">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="27bd8-270">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="27bd8-270">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="27bd8-271">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="27bd8-271">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="27bd8-272">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="27bd8-272">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="27bd8-273">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-273">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="27bd8-274">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-274">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="27bd8-275">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="27bd8-275">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="27bd8-276">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="27bd8-276">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="27bd8-277">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-277">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="27bd8-278">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="27bd8-278">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="27bd8-279">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-279">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="27bd8-280">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-280">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="27bd8-281">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-281">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="27bd8-282">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="27bd8-282">Supported configurations described next:</span></span>

* <span data-ttu-id="27bd8-283">無組態</span><span class="sxs-lookup"><span data-stu-id="27bd8-283">No configuration</span></span>
* <span data-ttu-id="27bd8-284">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="27bd8-284">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="27bd8-285">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="27bd8-285">Change the defaults in code</span></span>

<span data-ttu-id="27bd8-286">*無組態*</span><span class="sxs-lookup"><span data-stu-id="27bd8-286">*No configuration*</span></span>

<span data-ttu-id="27bd8-287">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-287">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="27bd8-288">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="27bd8-288">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="27bd8-289">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-289">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="27bd8-290">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="27bd8-290">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="27bd8-291">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="27bd8-291">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="27bd8-292">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="27bd8-292">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="27bd8-293">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-293">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="27bd8-294">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-294">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="27bd8-295">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-295">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="27bd8-296">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="27bd8-296">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="27bd8-297">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="27bd8-297">Schema notes:</span></span>

* <span data-ttu-id="27bd8-298">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="27bd8-298">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="27bd8-299">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="27bd8-299">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="27bd8-300">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="27bd8-300">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="27bd8-301">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="27bd8-301">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="27bd8-302">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-302">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="27bd8-303">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="27bd8-303">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="27bd8-304">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="27bd8-304">The `Certificate` section is optional.</span></span> <span data-ttu-id="27bd8-305">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="27bd8-305">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="27bd8-306">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="27bd8-306">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="27bd8-307">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-307">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="27bd8-308">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="27bd8-308">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="27bd8-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="27bd8-310">`KestrelServerOptions.ConfigurationLoader` 可以直接存取，以繼續逐一查看現有的載入器，例如 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所提供的載入器。</span><span class="sxs-lookup"><span data-stu-id="27bd8-310">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="27bd8-311">每個端點的 [設定] 區段都可以在 `Endpoint` 方法的選項中使用，如此一來，就可以讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-311">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="27bd8-312">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="27bd8-312">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="27bd8-313">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-313">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="27bd8-314">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="27bd8-314">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="27bd8-315">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="27bd8-315">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="27bd8-316">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="27bd8-316">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="27bd8-317">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="27bd8-317">*Change the defaults in code*</span></span>

<span data-ttu-id="27bd8-318">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-318">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="27bd8-319">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="27bd8-319">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="27bd8-320">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="27bd8-320">*Kestrel support for SNI*</span></span>

<span data-ttu-id="27bd8-321">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="27bd8-321">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="27bd8-322">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-322">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="27bd8-323">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="27bd8-323">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="27bd8-324">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="27bd8-324">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="27bd8-325">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-325">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="27bd8-326">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="27bd8-326">SNI support requires:</span></span>

* <span data-ttu-id="27bd8-327">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-327">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="27bd8-328">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律會 `null`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-328">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="27bd8-329">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-329">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="27bd8-330">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-330">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="27bd8-331">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-331">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="27bd8-332">連接記錄</span><span class="sxs-lookup"><span data-stu-id="27bd8-332">Connection logging</span></span>

<span data-ttu-id="27bd8-333">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>，針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="27bd8-333">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="27bd8-334">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="27bd8-334">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="27bd8-335">如果 `UseConnectionLogging` 放在 `UseHttps`之前，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="27bd8-335">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="27bd8-336">如果 `UseConnectionLogging` 放在 `UseHttps`之後，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="27bd8-336">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="27bd8-337">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="27bd8-337">Bind to a TCP socket</span></span>

<span data-ttu-id="27bd8-338"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-338">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="27bd8-339">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-339">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="27bd8-340">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="27bd8-340">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="27bd8-341">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="27bd8-341">Bind to a Unix socket</span></span>

<span data-ttu-id="27bd8-342">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="27bd8-342">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="27bd8-343">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="27bd8-343">Port 0</span></span>

<span data-ttu-id="27bd8-344">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-344">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="27bd8-345">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="27bd8-345">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="27bd8-346">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="27bd8-346">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="27bd8-347">限制</span><span class="sxs-lookup"><span data-stu-id="27bd8-347">Limitations</span></span>

<span data-ttu-id="27bd8-348">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="27bd8-348">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="27bd8-349">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="27bd8-349">`--urls` command-line argument</span></span>
* <span data-ttu-id="27bd8-350">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="27bd8-350">`urls` host configuration key</span></span>
* <span data-ttu-id="27bd8-351">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="27bd8-351">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="27bd8-352">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-352">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="27bd8-353">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="27bd8-353">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="27bd8-354">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-354">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="27bd8-355">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="27bd8-355">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="27bd8-356">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="27bd8-356">IIS endpoint configuration</span></span>

<span data-ttu-id="27bd8-357">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-357">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="27bd8-358">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="27bd8-358">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="27bd8-359">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="27bd8-359">ListenOptions.Protocols</span></span>

<span data-ttu-id="27bd8-360">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-360">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="27bd8-361">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="27bd8-361">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="27bd8-362">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="27bd8-362">`HttpProtocols` enum value</span></span> | <span data-ttu-id="27bd8-363">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="27bd8-363">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="27bd8-364">僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="27bd8-364">HTTP/1.1 only.</span></span> <span data-ttu-id="27bd8-365">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-365">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="27bd8-366">僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="27bd8-366">HTTP/2 only.</span></span> <span data-ttu-id="27bd8-367">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-367">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="27bd8-368">HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="27bd8-368">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="27bd8-369">HTTP/2 要求用戶端選取 TLS[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)交握中的 HTTP/2;否則，連接預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="27bd8-369">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="27bd8-370">任何端點的預設 `ListenOptions.Protocols` 值都會 `HttpProtocols.Http1AndHttp2`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-370">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="27bd8-371">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="27bd8-371">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="27bd8-372">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="27bd8-372">TLS version 1.2 or later</span></span>
* <span data-ttu-id="27bd8-373">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="27bd8-373">Renegotiation disabled</span></span>
* <span data-ttu-id="27bd8-374">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="27bd8-374">Compression disabled</span></span>
* <span data-ttu-id="27bd8-375">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="27bd8-375">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="27bd8-376">橢圓曲線 Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 最小 224 個位元</span><span class="sxs-lookup"><span data-stu-id="27bd8-376">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="27bd8-377">有限欄位 Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 最小 2048 個位元</span><span class="sxs-lookup"><span data-stu-id="27bd8-377">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="27bd8-378">加密套件未列於封鎖清單中</span><span class="sxs-lookup"><span data-stu-id="27bd8-378">Cipher suite not blacklisted</span></span>

<span data-ttu-id="27bd8-379">預設支援 `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; 與 P-256 橢圓曲線 &lbrack;`FIPS186`&rbrack;。</span><span class="sxs-lookup"><span data-stu-id="27bd8-379">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="27bd8-380">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="27bd8-380">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="27bd8-381">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="27bd8-381">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="27bd8-382">如有需要，請使用連線中介軟體，針對特定的加密依據每個連線來篩選 TLS 交握。</span><span class="sxs-lookup"><span data-stu-id="27bd8-382">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="27bd8-383">下列範例會針對應用程式不支援的任何加密演算法擲回 <xref:System.NotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="27bd8-383">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="27bd8-384">或者，定義 ITlsHandshakeFeature，並將[CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm)與可接受的加密套件清單進行比較。</span><span class="sxs-lookup"><span data-stu-id="27bd8-384">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="27bd8-385">不會使用加密來搭配[CipherAlgorithmType。 Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher 演算法。</span><span class="sxs-lookup"><span data-stu-id="27bd8-385">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

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

<span data-ttu-id="27bd8-386">您也可以透過 <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda 來設定連接篩選：</span><span class="sxs-lookup"><span data-stu-id="27bd8-386">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

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

<span data-ttu-id="27bd8-387">在 Linux 上，您可以使用 <xref:System.Net.Security.CipherSuitesPolicy>，依據每個連線來篩選 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="27bd8-387">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

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

<span data-ttu-id="27bd8-388">從設定進行通訊協定設定</span><span class="sxs-lookup"><span data-stu-id="27bd8-388">*Set the protocol from configuration*</span></span>

<span data-ttu-id="27bd8-389">`CreateDefaultBuilder` 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-389">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="27bd8-390">下列*appsettings*範例會建立 HTTP/1.1 作為所有端點的預設連接通訊協定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-390">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="27bd8-391">下列*appsettings*範例會建立特定端點的 HTTP/1.1 連接通訊協定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-391">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="27bd8-392">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="27bd8-392">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="27bd8-393">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="27bd8-393">Transport configuration</span></span>

<span data-ttu-id="27bd8-394">針對需要使用 Libuv （<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>）的專案：</span><span class="sxs-lookup"><span data-stu-id="27bd8-394">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="27bd8-395">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="27bd8-395">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="27bd8-396">在 `IWebHostBuilder` 上呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="27bd8-396">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="27bd8-397">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="27bd8-397">URL prefixes</span></span>

<span data-ttu-id="27bd8-398">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="27bd8-398">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="27bd8-399">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="27bd8-399">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="27bd8-400">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-400">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="27bd8-401">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-401">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="27bd8-402">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="27bd8-402">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="27bd8-403">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-403">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="27bd8-404">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="27bd8-404">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="27bd8-405">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-405">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="27bd8-406">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="27bd8-406">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="27bd8-407">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="27bd8-407">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="27bd8-408">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-408">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="27bd8-409">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-409">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="27bd8-410">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-410">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="27bd8-411">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="27bd8-411">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="27bd8-412">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="27bd8-412">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="27bd8-413">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="27bd8-413">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="27bd8-414">主機篩選</span><span class="sxs-lookup"><span data-stu-id="27bd8-414">Host filtering</span></span>

<span data-ttu-id="27bd8-415">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="27bd8-415">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="27bd8-416">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="27bd8-416">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="27bd8-417">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="27bd8-417">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="27bd8-418">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-418">`Host` headers aren't validated.</span></span>

<span data-ttu-id="27bd8-419">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="27bd8-419">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="27bd8-420">主機篩選中介軟體是由[AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件提供，這是針對 ASP.NET Core 應用程式而隱含提供的。</span><span class="sxs-lookup"><span data-stu-id="27bd8-420">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="27bd8-421">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="27bd8-421">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="27bd8-422">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="27bd8-422">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="27bd8-423">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="27bd8-423">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="27bd8-424">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="27bd8-424">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="27bd8-425">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="27bd8-425">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="27bd8-426">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="27bd8-426">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="27bd8-427">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="27bd8-427">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="27bd8-428">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-428">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="27bd8-429">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-429">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="27bd8-430">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="27bd8-430">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="27bd8-431">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-431">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="27bd8-432">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="27bd8-432">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="27bd8-433">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="27bd8-433">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="27bd8-434">HTTPS</span><span class="sxs-lookup"><span data-stu-id="27bd8-434">HTTPS</span></span>
* <span data-ttu-id="27bd8-435">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="27bd8-435">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="27bd8-436">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="27bd8-436">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="27bd8-437">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="27bd8-437">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="27bd8-438">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="27bd8-438">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="27bd8-439">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-439">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="27bd8-440">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="27bd8-440">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="27bd8-441">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="27bd8-441">HTTP/2 support</span></span>

<span data-ttu-id="27bd8-442">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="27bd8-442">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="27bd8-443">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="27bd8-443">Operating system&dagger;</span></span>
  * <span data-ttu-id="27bd8-444">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="27bd8-444">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="27bd8-445">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="27bd8-445">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="27bd8-446">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="27bd8-446">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="27bd8-447">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="27bd8-447">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="27bd8-448">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="27bd8-448">TLS 1.2 or later connection</span></span>

<span data-ttu-id="27bd8-449">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="27bd8-449">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="27bd8-450">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-450">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="27bd8-451">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-451">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="27bd8-452">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="27bd8-452">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="27bd8-453">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-453">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="27bd8-454">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="27bd8-454">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="27bd8-455">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="27bd8-455">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="27bd8-456">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="27bd8-456">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="27bd8-457">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-457">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="27bd8-458">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-458">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="27bd8-459">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="27bd8-459">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="27bd8-461">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="27bd8-461">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="27bd8-463">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-463">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="27bd8-464">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-464">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="27bd8-465">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="27bd8-465">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="27bd8-466">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-466">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="27bd8-467">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="27bd8-467">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="27bd8-468">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="27bd8-468">A reverse proxy:</span></span>

* <span data-ttu-id="27bd8-469">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="27bd8-469">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="27bd8-470">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="27bd8-470">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="27bd8-471">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="27bd8-471">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="27bd8-472">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="27bd8-472">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="27bd8-473">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="27bd8-473">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="27bd8-474">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-474">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="27bd8-475">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="27bd8-475">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="27bd8-476">[Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)套件包含在 AspNetCore 中。[應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-476">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="27bd8-477">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-477">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="27bd8-478">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="27bd8-478">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="27bd8-479">如需 `CreateDefaultBuilder` 和建立主機的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host> 的*設定主機*一節。</span><span class="sxs-lookup"><span data-stu-id="27bd8-479">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="27bd8-480">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請使用 `ConfigureKestrel`：</span><span class="sxs-lookup"><span data-stu-id="27bd8-480">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="27bd8-481">如果應用程式未呼叫 `CreateDefaultBuilder` 來設定主機，請在呼叫 `ConfigureKestrel` **之前**，先呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="27bd8-481">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="27bd8-482">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="27bd8-482">Kestrel options</span></span>

<span data-ttu-id="27bd8-483">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-483">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="27bd8-484">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="27bd8-484">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="27bd8-485">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="27bd8-485">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="27bd8-486">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="27bd8-486">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="27bd8-487">Kestrel 選項（在C#下列範例的程式碼中設定）也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-487">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="27bd8-488">例如，檔案設定提供者可以從*appsettings*或 Appsettings 載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="27bd8-488">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="27bd8-489">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="27bd8-489">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="27bd8-490">在 `Startup.ConfigureServices`中設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="27bd8-490">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="27bd8-491">將 `IConfiguration` 的實例插入 `Startup` 類別中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-491">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="27bd8-492">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="27bd8-492">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="27bd8-493">在 `Startup.ConfigureServices` 中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-493">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="27bd8-494">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="27bd8-494">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="27bd8-495">在*Program.cs*中，將設定的 [`Kestrel`] 區段載入 Kestrel 的設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-495">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="27bd8-496">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-496">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="27bd8-497">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="27bd8-497">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="27bd8-498">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-498">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="27bd8-499">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="27bd8-499">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="27bd8-500">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-500">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="27bd8-501">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="27bd8-501">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="27bd8-502">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-502">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="27bd8-503">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-503">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="27bd8-504">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-504">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="27bd8-505">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-505">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="27bd8-506">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="27bd8-506">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="27bd8-507">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="27bd8-507">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="27bd8-508">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="27bd8-508">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="27bd8-509">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-509">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="27bd8-510">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="27bd8-510">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="27bd8-511">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="27bd8-511">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="27bd8-512">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-512">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="27bd8-513">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="27bd8-513">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="27bd8-514">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="27bd8-514">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="27bd8-515">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="27bd8-515">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="27bd8-516">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="27bd8-516">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="27bd8-517">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="27bd8-517">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="27bd8-518">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="27bd8-518">A minimum rate also applies to the response.</span></span> <span data-ttu-id="27bd8-519">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="27bd8-519">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="27bd8-520">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="27bd8-520">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="27bd8-521">覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="27bd8-521">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="27bd8-522">因為通訊協定對要求多工的支援，所以 HTTP/2 不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的所有速率功能都不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-522">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="27bd8-523">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="27bd8-523">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="27bd8-524">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="27bd8-524">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="27bd8-525">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-525">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="27bd8-526">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="27bd8-526">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="27bd8-527">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-527">Maximum streams per connection</span></span>

<span data-ttu-id="27bd8-528">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="27bd8-528">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="27bd8-529">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="27bd8-529">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="27bd8-530">預設值為 100。</span><span class="sxs-lookup"><span data-stu-id="27bd8-530">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="27bd8-531">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="27bd8-531">Header table size</span></span>

<span data-ttu-id="27bd8-532">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="27bd8-532">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="27bd8-533">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="27bd8-533">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="27bd8-534">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-534">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="27bd8-535">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="27bd8-535">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="27bd8-536">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-536">Maximum frame size</span></span>

<span data-ttu-id="27bd8-537">`Http2.MaxFrameSize` 表示要接收的 HTTP/2 連線框架承載大小上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-537">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="27bd8-538">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="27bd8-538">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="27bd8-539">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-539">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="27bd8-540">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-540">Maximum request header size</span></span>

<span data-ttu-id="27bd8-541">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-541">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="27bd8-542">此限制皆共同套用至已壓縮及未壓縮代表中的名稱與值。</span><span class="sxs-lookup"><span data-stu-id="27bd8-542">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="27bd8-543">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-543">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="27bd8-544">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="27bd8-544">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="27bd8-545">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="27bd8-545">Initial connection window size</span></span>

<span data-ttu-id="27bd8-546">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-546">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="27bd8-547">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-547">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="27bd8-548">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-548">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="27bd8-549">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-549">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="27bd8-550">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="27bd8-550">Initial stream window size</span></span>

<span data-ttu-id="27bd8-551">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-551">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="27bd8-552">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-552">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="27bd8-553">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-553">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="27bd8-554">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-554">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="27bd8-555">同步 IO</span><span class="sxs-lookup"><span data-stu-id="27bd8-555">Synchronous IO</span></span>

<span data-ttu-id="27bd8-556"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> 控制是否允許要求與回應的同步 IO。</span><span class="sxs-lookup"><span data-stu-id="27bd8-556"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="27bd8-557">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-557">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="27bd8-558">大量的封鎖同步 IO 作業會導致執行緒集區耗盡，這會使得應用程式沒有回應。</span><span class="sxs-lookup"><span data-stu-id="27bd8-558">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="27bd8-559">只有當使用不支援同步 IO 的程式庫時才啟用 `AllowSynchronousIO`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-559">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="27bd8-560">下列範例會啟用同步 IO：</span><span class="sxs-lookup"><span data-stu-id="27bd8-560">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="27bd8-561">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="27bd8-561">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="27bd8-562">端點組態</span><span class="sxs-lookup"><span data-stu-id="27bd8-562">Endpoint configuration</span></span>

<span data-ttu-id="27bd8-563">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="27bd8-563">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="27bd8-564">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="27bd8-564">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="27bd8-565">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="27bd8-565">Specify URLs using the:</span></span>

* <span data-ttu-id="27bd8-566">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="27bd8-566">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="27bd8-567">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="27bd8-567">`--urls` command-line argument.</span></span>
* <span data-ttu-id="27bd8-568">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="27bd8-568">`urls` host configuration key.</span></span>
* <span data-ttu-id="27bd8-569">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="27bd8-569">`UseUrls` extension method.</span></span>

<span data-ttu-id="27bd8-570">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-570">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="27bd8-571">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000; http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-571">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="27bd8-572">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-572">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="27bd8-573">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="27bd8-573">A development certificate is created:</span></span>

* <span data-ttu-id="27bd8-574">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="27bd8-574">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="27bd8-575">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-575">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="27bd8-576">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-576">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="27bd8-577">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-577">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="27bd8-578">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-578">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="27bd8-579">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-579">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="27bd8-580">`KestrelServerOptions` 設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-580">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="27bd8-581">ConfigureEndpointDefaults （Action\<Listenoptions 來 >）</span><span class="sxs-lookup"><span data-stu-id="27bd8-581">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="27bd8-582">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-582">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="27bd8-583">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-583">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="27bd8-584">ConfigureHttpsDefaults （Action\<HttpsConnectionAdapterOptions >）</span><span class="sxs-lookup"><span data-stu-id="27bd8-584">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="27bd8-585">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-585">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="27bd8-586">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-586">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="27bd8-587">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="27bd8-587">Configure(IConfiguration)</span></span>

<span data-ttu-id="27bd8-588">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-588">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="27bd8-589">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="27bd8-589">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="27bd8-590">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="27bd8-590">ListenOptions.UseHttps</span></span>

<span data-ttu-id="27bd8-591">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-591">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="27bd8-592">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="27bd8-592">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="27bd8-593">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-593">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="27bd8-594">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="27bd8-594">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="27bd8-595">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="27bd8-595">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="27bd8-596">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="27bd8-596">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="27bd8-597">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="27bd8-597">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="27bd8-598">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-598">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="27bd8-599">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-599">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="27bd8-600">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="27bd8-600">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="27bd8-601">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="27bd8-601">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="27bd8-602">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-602">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="27bd8-603">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="27bd8-603">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="27bd8-604">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-604">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="27bd8-605">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-605">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="27bd8-606">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-606">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="27bd8-607">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="27bd8-607">Supported configurations described next:</span></span>

* <span data-ttu-id="27bd8-608">無組態</span><span class="sxs-lookup"><span data-stu-id="27bd8-608">No configuration</span></span>
* <span data-ttu-id="27bd8-609">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="27bd8-609">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="27bd8-610">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="27bd8-610">Change the defaults in code</span></span>

<span data-ttu-id="27bd8-611">*無組態*</span><span class="sxs-lookup"><span data-stu-id="27bd8-611">*No configuration*</span></span>

<span data-ttu-id="27bd8-612">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-612">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="27bd8-613">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="27bd8-613">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="27bd8-614">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-614">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="27bd8-615">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="27bd8-615">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="27bd8-616">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="27bd8-616">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="27bd8-617">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="27bd8-617">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="27bd8-618">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-618">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="27bd8-619">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-619">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="27bd8-620">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-620">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="27bd8-621">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="27bd8-621">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="27bd8-622">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="27bd8-622">Schema notes:</span></span>

* <span data-ttu-id="27bd8-623">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="27bd8-623">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="27bd8-624">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="27bd8-624">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="27bd8-625">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="27bd8-625">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="27bd8-626">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="27bd8-626">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="27bd8-627">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-627">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="27bd8-628">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="27bd8-628">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="27bd8-629">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="27bd8-629">The `Certificate` section is optional.</span></span> <span data-ttu-id="27bd8-630">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="27bd8-630">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="27bd8-631">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="27bd8-631">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="27bd8-632">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-632">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="27bd8-633">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="27bd8-633">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="27bd8-634">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-634">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="27bd8-635">`KestrelServerOptions.ConfigurationLoader` 可以直接存取，以繼續逐一查看現有的載入器，例如 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所提供的載入器。</span><span class="sxs-lookup"><span data-stu-id="27bd8-635">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="27bd8-636">每個端點的 [設定] 區段都可以在 `Endpoint` 方法的選項中使用，如此一來，就可以讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-636">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="27bd8-637">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="27bd8-637">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="27bd8-638">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-638">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="27bd8-639">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="27bd8-639">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="27bd8-640">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="27bd8-640">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="27bd8-641">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="27bd8-641">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="27bd8-642">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="27bd8-642">*Change the defaults in code*</span></span>

<span data-ttu-id="27bd8-643">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-643">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="27bd8-644">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="27bd8-644">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="27bd8-645">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="27bd8-645">*Kestrel support for SNI*</span></span>

<span data-ttu-id="27bd8-646">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="27bd8-646">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="27bd8-647">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-647">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="27bd8-648">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="27bd8-648">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="27bd8-649">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="27bd8-649">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="27bd8-650">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-650">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="27bd8-651">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="27bd8-651">SNI support requires:</span></span>

* <span data-ttu-id="27bd8-652">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-652">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="27bd8-653">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律會 `null`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-653">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="27bd8-654">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-654">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="27bd8-655">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-655">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="27bd8-656">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-656">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="27bd8-657">連接記錄</span><span class="sxs-lookup"><span data-stu-id="27bd8-657">Connection logging</span></span>

<span data-ttu-id="27bd8-658">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>，針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="27bd8-658">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="27bd8-659">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="27bd8-659">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="27bd8-660">如果 `UseConnectionLogging` 放在 `UseHttps`之前，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="27bd8-660">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="27bd8-661">如果 `UseConnectionLogging` 放在 `UseHttps`之後，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="27bd8-661">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="27bd8-662">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="27bd8-662">Bind to a TCP socket</span></span>

<span data-ttu-id="27bd8-663"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-663">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="27bd8-664">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-664">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="27bd8-665">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="27bd8-665">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="27bd8-666">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="27bd8-666">Bind to a Unix socket</span></span>

<span data-ttu-id="27bd8-667">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="27bd8-667">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="27bd8-668">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="27bd8-668">Port 0</span></span>

<span data-ttu-id="27bd8-669">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-669">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="27bd8-670">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="27bd8-670">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="27bd8-671">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="27bd8-671">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="27bd8-672">限制</span><span class="sxs-lookup"><span data-stu-id="27bd8-672">Limitations</span></span>

<span data-ttu-id="27bd8-673">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="27bd8-673">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="27bd8-674">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="27bd8-674">`--urls` command-line argument</span></span>
* <span data-ttu-id="27bd8-675">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="27bd8-675">`urls` host configuration key</span></span>
* <span data-ttu-id="27bd8-676">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="27bd8-676">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="27bd8-677">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-677">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="27bd8-678">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="27bd8-678">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="27bd8-679">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-679">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="27bd8-680">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="27bd8-680">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="27bd8-681">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="27bd8-681">IIS endpoint configuration</span></span>

<span data-ttu-id="27bd8-682">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-682">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="27bd8-683">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="27bd8-683">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="27bd8-684">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="27bd8-684">ListenOptions.Protocols</span></span>

<span data-ttu-id="27bd8-685">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-685">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="27bd8-686">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="27bd8-686">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="27bd8-687">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="27bd8-687">`HttpProtocols` enum value</span></span> | <span data-ttu-id="27bd8-688">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="27bd8-688">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="27bd8-689">僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="27bd8-689">HTTP/1.1 only.</span></span> <span data-ttu-id="27bd8-690">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-690">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="27bd8-691">僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="27bd8-691">HTTP/2 only.</span></span> <span data-ttu-id="27bd8-692">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-692">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="27bd8-693">HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="27bd8-693">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="27bd8-694">HTTP/2 需要 TLS 和[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)連線;否則，連接預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="27bd8-694">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="27bd8-695">預設通訊協定為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="27bd8-695">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="27bd8-696">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="27bd8-696">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="27bd8-697">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="27bd8-697">TLS version 1.2 or later</span></span>
* <span data-ttu-id="27bd8-698">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="27bd8-698">Renegotiation disabled</span></span>
* <span data-ttu-id="27bd8-699">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="27bd8-699">Compression disabled</span></span>
* <span data-ttu-id="27bd8-700">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="27bd8-700">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="27bd8-701">橢圓曲線 Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 最小 224 個位元</span><span class="sxs-lookup"><span data-stu-id="27bd8-701">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="27bd8-702">有限欄位 Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 最小 2048 個位元</span><span class="sxs-lookup"><span data-stu-id="27bd8-702">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="27bd8-703">加密套件未列於封鎖清單中</span><span class="sxs-lookup"><span data-stu-id="27bd8-703">Cipher suite not blacklisted</span></span>

<span data-ttu-id="27bd8-704">預設支援 `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; 與 P-256 橢圓曲線 &lbrack;`FIPS186`&rbrack;。</span><span class="sxs-lookup"><span data-stu-id="27bd8-704">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="27bd8-705">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="27bd8-705">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="27bd8-706">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="27bd8-706">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="27bd8-707">選擇性地建立 `IConnectionAdapter` 實作，針對每個連線來篩選特定加密的 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="27bd8-707">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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

<span data-ttu-id="27bd8-708">從設定進行通訊協定設定</span><span class="sxs-lookup"><span data-stu-id="27bd8-708">*Set the protocol from configuration*</span></span>

<span data-ttu-id="27bd8-709"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-709"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="27bd8-710">在下列 *appsettings.json* 範例中，會為 Kestrel 的所有端點建立預設連線通訊協定 (HTTP/1.1 和 HTTP/2)：</span><span class="sxs-lookup"><span data-stu-id="27bd8-710">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="27bd8-711">下列設定檔範例會為特定端點建立連線通訊協定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-711">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="27bd8-712">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="27bd8-712">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="27bd8-713">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="27bd8-713">Transport configuration</span></span>

<span data-ttu-id="27bd8-714">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="27bd8-714">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="27bd8-715">對於升級到 2.1 且會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> 並相依於下列任一套件的 ASP.NET Core 2.0 應用程式來說，這是一項中斷性變更：</span><span class="sxs-lookup"><span data-stu-id="27bd8-715">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="27bd8-716">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="27bd8-716">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="27bd8-717">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="27bd8-717">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="27bd8-718">針對需要使用 Libuv 的專案：</span><span class="sxs-lookup"><span data-stu-id="27bd8-718">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="27bd8-719">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="27bd8-719">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="27bd8-720">呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="27bd8-720">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="27bd8-721">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="27bd8-721">URL prefixes</span></span>

<span data-ttu-id="27bd8-722">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="27bd8-722">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="27bd8-723">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="27bd8-723">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="27bd8-724">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-724">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="27bd8-725">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-725">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="27bd8-726">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="27bd8-726">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="27bd8-727">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-727">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="27bd8-728">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="27bd8-728">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="27bd8-729">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-729">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="27bd8-730">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="27bd8-730">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="27bd8-731">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="27bd8-731">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="27bd8-732">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-732">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="27bd8-733">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-733">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="27bd8-734">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-734">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="27bd8-735">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="27bd8-735">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="27bd8-736">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="27bd8-736">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="27bd8-737">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="27bd8-737">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="27bd8-738">主機篩選</span><span class="sxs-lookup"><span data-stu-id="27bd8-738">Host filtering</span></span>

<span data-ttu-id="27bd8-739">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="27bd8-739">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="27bd8-740">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="27bd8-740">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="27bd8-741">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="27bd8-741">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="27bd8-742">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-742">`Host` headers aren't validated.</span></span>

<span data-ttu-id="27bd8-743">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="27bd8-743">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="27bd8-744">主機篩選中介軟體是由[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)（ASP.NET Core 2.1 或2.2）所包含的[HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件所提供。</span><span class="sxs-lookup"><span data-stu-id="27bd8-744">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="27bd8-745">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="27bd8-745">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="27bd8-746">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="27bd8-746">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="27bd8-747">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="27bd8-747">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="27bd8-748">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="27bd8-748">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="27bd8-749">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="27bd8-749">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="27bd8-750">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="27bd8-750">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="27bd8-751">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="27bd8-751">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="27bd8-752">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-752">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="27bd8-753">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-753">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="27bd8-754">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="27bd8-754">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="27bd8-755">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-755">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="27bd8-756">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="27bd8-756">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="27bd8-757">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="27bd8-757">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="27bd8-758">HTTPS</span><span class="sxs-lookup"><span data-stu-id="27bd8-758">HTTPS</span></span>
* <span data-ttu-id="27bd8-759">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="27bd8-759">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="27bd8-760">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="27bd8-760">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="27bd8-761">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-761">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="27bd8-762">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="27bd8-762">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="27bd8-763">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="27bd8-763">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="27bd8-764">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」搭配使用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-764">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="27bd8-765">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-765">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="27bd8-766">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="27bd8-766">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="27bd8-768">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="27bd8-768">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="27bd8-770">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-770">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="27bd8-771">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-771">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="27bd8-772">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="27bd8-772">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="27bd8-773">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-773">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="27bd8-774">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="27bd8-774">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="27bd8-775">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="27bd8-775">A reverse proxy:</span></span>

* <span data-ttu-id="27bd8-776">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="27bd8-776">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="27bd8-777">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="27bd8-777">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="27bd8-778">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="27bd8-778">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="27bd8-779">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="27bd8-779">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="27bd8-780">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="27bd8-780">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="27bd8-781">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-781">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="27bd8-782">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="27bd8-782">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="27bd8-783">[Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)套件包含在 AspNetCore 中。[應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-783">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="27bd8-784">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-784">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="27bd8-785">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="27bd8-785">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="27bd8-786">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="27bd8-786">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="27bd8-787">如需 `CreateDefaultBuilder` 和建立主機的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host> 的*設定主機*一節。</span><span class="sxs-lookup"><span data-stu-id="27bd8-787">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="27bd8-788">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="27bd8-788">Kestrel options</span></span>

<span data-ttu-id="27bd8-789">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-789">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="27bd8-790">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="27bd8-790">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="27bd8-791">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="27bd8-791">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="27bd8-792">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="27bd8-792">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="27bd8-793">Kestrel 選項（在C#下列範例的程式碼中設定）也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-793">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="27bd8-794">例如，檔案設定提供者可以從*appsettings*或 Appsettings 載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="27bd8-794">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="27bd8-795">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="27bd8-795">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="27bd8-796">在 `Startup.ConfigureServices`中設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="27bd8-796">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="27bd8-797">將 `IConfiguration` 的實例插入 `Startup` 類別中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-797">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="27bd8-798">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="27bd8-798">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="27bd8-799">在 `Startup.ConfigureServices` 中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-799">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="27bd8-800">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="27bd8-800">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="27bd8-801">在*Program.cs*中，將設定的 [`Kestrel`] 區段載入 Kestrel 的設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-801">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="27bd8-802">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-802">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="27bd8-803">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="27bd8-803">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="27bd8-804">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-804">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="27bd8-805">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="27bd8-805">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="27bd8-806">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-806">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="27bd8-807">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="27bd8-807">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="27bd8-808">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-808">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="27bd8-809">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-809">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="27bd8-810">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-810">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="27bd8-811">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="27bd8-811">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="27bd8-812">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="27bd8-812">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="27bd8-813">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="27bd8-813">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="27bd8-814">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="27bd8-814">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="27bd8-815">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-815">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="27bd8-816">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="27bd8-816">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="27bd8-817">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="27bd8-817">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="27bd8-818">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="27bd8-818">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="27bd8-819">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="27bd8-819">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="27bd8-820">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="27bd8-820">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="27bd8-821">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="27bd8-821">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="27bd8-822">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="27bd8-822">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="27bd8-823">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="27bd8-823">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="27bd8-824">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="27bd8-824">A minimum rate also applies to the response.</span></span> <span data-ttu-id="27bd8-825">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="27bd8-825">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="27bd8-826">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="27bd8-826">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="27bd8-827">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="27bd8-827">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="27bd8-828">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="27bd8-828">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="27bd8-829">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="27bd8-829">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="27bd8-830">同步 IO</span><span class="sxs-lookup"><span data-stu-id="27bd8-830">Synchronous IO</span></span>

<span data-ttu-id="27bd8-831"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> 控制是否允許要求與回應的同步 IO。</span><span class="sxs-lookup"><span data-stu-id="27bd8-831"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="27bd8-832">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-832">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="27bd8-833">大量的封鎖同步 IO 作業會導致執行緒集區耗盡，這會使得應用程式沒有回應。</span><span class="sxs-lookup"><span data-stu-id="27bd8-833">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="27bd8-834">只有當使用不支援同步 IO 的程式庫時才啟用 `AllowSynchronousIO`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-834">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="27bd8-835">下列範例會停用同步 IO：</span><span class="sxs-lookup"><span data-stu-id="27bd8-835">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="27bd8-836">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="27bd8-836">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="27bd8-837">端點組態</span><span class="sxs-lookup"><span data-stu-id="27bd8-837">Endpoint configuration</span></span>

<span data-ttu-id="27bd8-838">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="27bd8-838">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="27bd8-839">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="27bd8-839">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="27bd8-840">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="27bd8-840">Specify URLs using the:</span></span>

* <span data-ttu-id="27bd8-841">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="27bd8-841">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="27bd8-842">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="27bd8-842">`--urls` command-line argument.</span></span>
* <span data-ttu-id="27bd8-843">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="27bd8-843">`urls` host configuration key.</span></span>
* <span data-ttu-id="27bd8-844">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="27bd8-844">`UseUrls` extension method.</span></span>

<span data-ttu-id="27bd8-845">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-845">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="27bd8-846">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000; http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-846">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="27bd8-847">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-847">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="27bd8-848">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="27bd8-848">A development certificate is created:</span></span>

* <span data-ttu-id="27bd8-849">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="27bd8-849">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="27bd8-850">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-850">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="27bd8-851">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-851">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="27bd8-852">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-852">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="27bd8-853">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-853">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="27bd8-854">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-854">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="27bd8-855">`KestrelServerOptions` 設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-855">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="27bd8-856">ConfigureEndpointDefaults （Action\<Listenoptions 來 >）</span><span class="sxs-lookup"><span data-stu-id="27bd8-856">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="27bd8-857">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-857">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="27bd8-858">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-858">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="27bd8-859">ConfigureHttpsDefaults （Action\<HttpsConnectionAdapterOptions >）</span><span class="sxs-lookup"><span data-stu-id="27bd8-859">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="27bd8-860">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-860">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="27bd8-861">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-861">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="27bd8-862">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="27bd8-862">Configure(IConfiguration)</span></span>

<span data-ttu-id="27bd8-863">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="27bd8-863">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="27bd8-864">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="27bd8-864">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="27bd8-865">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="27bd8-865">ListenOptions.UseHttps</span></span>

<span data-ttu-id="27bd8-866">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-866">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="27bd8-867">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="27bd8-867">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="27bd8-868">`UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-868">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="27bd8-869">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="27bd8-869">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="27bd8-870">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="27bd8-870">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="27bd8-871">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="27bd8-871">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="27bd8-872">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="27bd8-872">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="27bd8-873">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-873">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="27bd8-874">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-874">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="27bd8-875">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="27bd8-875">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="27bd8-876">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="27bd8-876">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="27bd8-877">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-877">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="27bd8-878">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="27bd8-878">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="27bd8-879">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-879">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="27bd8-880">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-880">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="27bd8-881">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-881">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="27bd8-882">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="27bd8-882">Supported configurations described next:</span></span>

* <span data-ttu-id="27bd8-883">無組態</span><span class="sxs-lookup"><span data-stu-id="27bd8-883">No configuration</span></span>
* <span data-ttu-id="27bd8-884">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="27bd8-884">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="27bd8-885">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="27bd8-885">Change the defaults in code</span></span>

<span data-ttu-id="27bd8-886">*無組態*</span><span class="sxs-lookup"><span data-stu-id="27bd8-886">*No configuration*</span></span>

<span data-ttu-id="27bd8-887">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-887">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="27bd8-888">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="27bd8-888">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="27bd8-889">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-889">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="27bd8-890">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="27bd8-890">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="27bd8-891">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="27bd8-891">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="27bd8-892">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="27bd8-892">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="27bd8-893">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-893">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="27bd8-894">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-894">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="27bd8-895">除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-895">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="27bd8-896">例如，[憑證] > [預設] 憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="27bd8-896">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="27bd8-897">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="27bd8-897">Schema notes:</span></span>

* <span data-ttu-id="27bd8-898">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="27bd8-898">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="27bd8-899">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="27bd8-899">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="27bd8-900">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="27bd8-900">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="27bd8-901">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="27bd8-901">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="27bd8-902">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="27bd8-902">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="27bd8-903">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="27bd8-903">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="27bd8-904">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="27bd8-904">The `Certificate` section is optional.</span></span> <span data-ttu-id="27bd8-905">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="27bd8-905">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="27bd8-906">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="27bd8-906">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="27bd8-907">`Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-907">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="27bd8-908">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="27bd8-908">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="27bd8-909">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-909">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="27bd8-910">`KestrelServerOptions.ConfigurationLoader` 可以直接存取，以繼續逐一查看現有的載入器，例如 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所提供的載入器。</span><span class="sxs-lookup"><span data-stu-id="27bd8-910">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="27bd8-911">每個端點的 [設定] 區段都可以在 `Endpoint` 方法的選項中使用，如此一來，就可以讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-911">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="27bd8-912">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="27bd8-912">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="27bd8-913">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-913">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="27bd8-914">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="27bd8-914">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="27bd8-915">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="27bd8-915">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="27bd8-916">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="27bd8-916">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="27bd8-917">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="27bd8-917">*Change the defaults in code*</span></span>

<span data-ttu-id="27bd8-918">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-918">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="27bd8-919">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="27bd8-919">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="27bd8-920">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="27bd8-920">*Kestrel support for SNI*</span></span>

<span data-ttu-id="27bd8-921">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="27bd8-921">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="27bd8-922">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-922">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="27bd8-923">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="27bd8-923">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="27bd8-924">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="27bd8-924">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="27bd8-925">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-925">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="27bd8-926">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="27bd8-926">SNI support requires:</span></span>

* <span data-ttu-id="27bd8-927">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-927">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="27bd8-928">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律會 `null`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-928">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="27bd8-929">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-929">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="27bd8-930">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="27bd8-930">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="27bd8-931">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-931">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="27bd8-932">連接記錄</span><span class="sxs-lookup"><span data-stu-id="27bd8-932">Connection logging</span></span>

<span data-ttu-id="27bd8-933">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>，針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="27bd8-933">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="27bd8-934">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="27bd8-934">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="27bd8-935">如果 `UseConnectionLogging` 放在 `UseHttps`之前，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="27bd8-935">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="27bd8-936">如果 `UseConnectionLogging` 放在 `UseHttps`之後，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="27bd8-936">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="27bd8-937">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="27bd8-937">Bind to a TCP socket</span></span>

<span data-ttu-id="27bd8-938"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="27bd8-938">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="27bd8-939">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-939">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="27bd8-940">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="27bd8-940">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="27bd8-941">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="27bd8-941">Bind to a Unix socket</span></span>

<span data-ttu-id="27bd8-942">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="27bd8-942">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

### <a name="port-0"></a><span data-ttu-id="27bd8-943">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="27bd8-943">Port 0</span></span>

<span data-ttu-id="27bd8-944">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="27bd8-944">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="27bd8-945">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="27bd8-945">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="27bd8-946">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="27bd8-946">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="27bd8-947">限制</span><span class="sxs-lookup"><span data-stu-id="27bd8-947">Limitations</span></span>

<span data-ttu-id="27bd8-948">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="27bd8-948">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="27bd8-949">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="27bd8-949">`--urls` command-line argument</span></span>
* <span data-ttu-id="27bd8-950">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="27bd8-950">`urls` host configuration key</span></span>
* <span data-ttu-id="27bd8-951">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="27bd8-951">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="27bd8-952">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="27bd8-952">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="27bd8-953">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="27bd8-953">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="27bd8-954">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-954">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="27bd8-955">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="27bd8-955">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="27bd8-956">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="27bd8-956">IIS endpoint configuration</span></span>

<span data-ttu-id="27bd8-957">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="27bd8-957">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="27bd8-958">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="27bd8-958">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="27bd8-959">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="27bd8-959">Transport configuration</span></span>

<span data-ttu-id="27bd8-960">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="27bd8-960">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="27bd8-961">對於升級到 2.1 且會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> 並相依於下列任一套件的 ASP.NET Core 2.0 應用程式來說，這是一項中斷性變更：</span><span class="sxs-lookup"><span data-stu-id="27bd8-961">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="27bd8-962">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="27bd8-962">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="27bd8-963">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="27bd8-963">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="27bd8-964">針對需要使用 Libuv 的專案：</span><span class="sxs-lookup"><span data-stu-id="27bd8-964">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="27bd8-965">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="27bd8-965">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="27bd8-966">呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="27bd8-966">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="27bd8-967">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="27bd8-967">URL prefixes</span></span>

<span data-ttu-id="27bd8-968">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="27bd8-968">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="27bd8-969">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="27bd8-969">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="27bd8-970">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="27bd8-970">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="27bd8-971">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-971">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="27bd8-972">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="27bd8-972">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="27bd8-973">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-973">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="27bd8-974">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="27bd8-974">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="27bd8-975">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-975">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="27bd8-976">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="27bd8-976">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="27bd8-977">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="27bd8-977">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="27bd8-978">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-978">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="27bd8-979">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="27bd8-979">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="27bd8-980">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="27bd8-980">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="27bd8-981">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="27bd8-981">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="27bd8-982">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="27bd8-982">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="27bd8-983">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="27bd8-983">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="27bd8-984">主機篩選</span><span class="sxs-lookup"><span data-stu-id="27bd8-984">Host filtering</span></span>

<span data-ttu-id="27bd8-985">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="27bd8-985">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="27bd8-986">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="27bd8-986">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="27bd8-987">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="27bd8-987">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="27bd8-988">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="27bd8-988">`Host` headers aren't validated.</span></span>

<span data-ttu-id="27bd8-989">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="27bd8-989">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="27bd8-990">主機篩選中介軟體是由[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)（ASP.NET Core 2.1 或2.2）所包含的[HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件所提供。</span><span class="sxs-lookup"><span data-stu-id="27bd8-990">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="27bd8-991">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="27bd8-991">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="27bd8-992">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="27bd8-992">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="27bd8-993">若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="27bd8-993">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="27bd8-994">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="27bd8-994">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="27bd8-995">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="27bd8-995">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="27bd8-996">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="27bd8-996">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="27bd8-997">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="27bd8-997">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="27bd8-998">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-998">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="27bd8-999">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="27bd8-999">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="27bd8-1000">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="27bd8-1000">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="27bd8-1001">其他資源</span><span class="sxs-lookup"><span data-stu-id="27bd8-1001">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="27bd8-1002">RFC 7230：訊息語法和路由 (第 5.4 節：主機)</span><span class="sxs-lookup"><span data-stu-id="27bd8-1002">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
