---
<span data-ttu-id="a545b-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-102">'Blazor'</span></span>
- <span data-ttu-id="a545b-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-103">'Identity'</span></span>
- <span data-ttu-id="a545b-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-105">'Razor'</span></span>
- <span data-ttu-id="a545b-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-106">'SignalR' uid:</span></span> 

---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="a545b-107">ASP.NET Core 中的 Kestrel 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="a545b-107">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="a545b-108">作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="a545b-108">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a545b-109">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="a545b-109">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="a545b-110">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="a545b-110">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="a545b-111">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="a545b-111">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="a545b-112">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a545b-112">HTTPS</span></span>
* <span data-ttu-id="a545b-113">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="a545b-113">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="a545b-114">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="a545b-114">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="a545b-115">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="a545b-115">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="a545b-116">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a545b-116">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="a545b-117">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-117">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="a545b-118">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a545b-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="a545b-119">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="a545b-119">HTTP/2 support</span></span>

<span data-ttu-id="a545b-120">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="a545b-120">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="a545b-121">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="a545b-121">Operating system&dagger;</span></span>
  * <span data-ttu-id="a545b-122">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="a545b-122">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="a545b-123">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="a545b-123">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="a545b-124">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a545b-124">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="a545b-125">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="a545b-125">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="a545b-126">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="a545b-126">TLS 1.2 or later connection</span></span>

<span data-ttu-id="a545b-127">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a545b-127">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="a545b-128">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="a545b-128">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="a545b-129">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="a545b-129">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="a545b-130">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="a545b-130">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="a545b-131">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="a545b-131">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="a545b-132">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a545b-132">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="a545b-133">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="a545b-133">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="a545b-134">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="a545b-134">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="a545b-135">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」\*\* 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="a545b-135">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="a545b-136">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-136">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="a545b-137">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="a545b-137">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="a545b-139">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="a545b-139">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a545b-141">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-141">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="a545b-142">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-142">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="a545b-143">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="a545b-143">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="a545b-144">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-144">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="a545b-145">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="a545b-145">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="a545b-146">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="a545b-146">A reverse proxy:</span></span>

* <span data-ttu-id="a545b-147">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="a545b-147">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="a545b-148">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="a545b-148">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="a545b-149">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="a545b-149">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="a545b-150">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="a545b-150">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="a545b-151">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="a545b-151">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="a545b-152">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="a545b-152">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="kestrel-in-aspnet-core-apps"></a><span data-ttu-id="a545b-153">ASP.NET Core 應用程式中的 Kestrel</span><span class="sxs-lookup"><span data-stu-id="a545b-153">Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="a545b-154">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-154">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="a545b-155">在*Program.cs*中， <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> 方法會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> ：</span><span class="sxs-lookup"><span data-stu-id="a545b-155">In *Program.cs*, the <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> method calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=8)]

<span data-ttu-id="a545b-156">如需建立主機的詳細資訊，請參閱的*設定主機*和預設產生器*設定*章節 <xref:fundamentals/host/generic-host#set-up-a-host> 。</span><span class="sxs-lookup"><span data-stu-id="a545b-156">For more information on building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="a545b-157">若要在呼叫 `ConfigureWebHostDefaults` 之後提供額外的設定，請使用 `ConfigureKestrel`：</span><span class="sxs-lookup"><span data-stu-id="a545b-157">To provide additional configuration after calling `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="a545b-158">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="a545b-158">Kestrel options</span></span>

<span data-ttu-id="a545b-159">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="a545b-159">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="a545b-160">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="a545b-160">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="a545b-161">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="a545b-161">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="a545b-162">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="a545b-162">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="a545b-163">在本文稍後所示的範例中，Kestrel 選項是在 c # 程式碼中設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-163">In examples shown later in this article, Kestrel options are configured in C# code.</span></span> <span data-ttu-id="a545b-164">您也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定 Kestrel 選項。</span><span class="sxs-lookup"><span data-stu-id="a545b-164">Kestrel options can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="a545b-165">例如，檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)可以從*appsettings*或 appsettings 載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="a545b-165">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider) can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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
> <span data-ttu-id="a545b-166"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>和[端點](#endpoint-configuration)設定可從設定提供者進行設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-166"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> and [endpoint configuration](#endpoint-configuration) are configurable from configuration providers.</span></span> <span data-ttu-id="a545b-167">其餘的 Kestrel 設定必須以 c # 程式碼進行設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-167">Remaining Kestrel configuration must be configured in C# code.</span></span>

<span data-ttu-id="a545b-168">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="a545b-168">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="a545b-169">在中設定 Kestrel `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="a545b-169">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="a545b-170">將的實例插入 `IConfiguration` 至 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="a545b-170">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="a545b-171">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a545b-171">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="a545b-172">在中 `Startup.ConfigureServices` ，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="a545b-172">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

* <span data-ttu-id="a545b-173">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="a545b-173">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="a545b-174">在*Program.cs*中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="a545b-174">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="a545b-175">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="a545b-175">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="a545b-176">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="a545b-176">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="a545b-177">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="a545b-177">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="a545b-178">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="a545b-178">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="a545b-179">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="a545b-179">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="a545b-180">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="a545b-180">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="a545b-181">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-181">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="a545b-182">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-182">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="a545b-183">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="a545b-183">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="a545b-184">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="a545b-184">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="a545b-185">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="a545b-185">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="a545b-186">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="a545b-186">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="a545b-187">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="a545b-187">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="a545b-188">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="a545b-188">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="a545b-189">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="a545b-189">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="a545b-190">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="a545b-190">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="a545b-191">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-191">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="a545b-192">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="a545b-192">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="a545b-193">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="a545b-193">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="a545b-194">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="a545b-194">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="a545b-195">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="a545b-195">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="a545b-196">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="a545b-196">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="a545b-197">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="a545b-197">A minimum rate also applies to the response.</span></span> <span data-ttu-id="a545b-198">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="a545b-198">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="a545b-199">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="a545b-199">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="a545b-200">覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="a545b-200">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="a545b-201">因為通訊協定對要求多工的支援，所以 HTTP/2 一般不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> 不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="a545b-201">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="a545b-202">不過，<xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> 仍存在 HTTP/2 要求的 `HttpContext.Features`您仍能透過將 `IHttpMinRequestBodyDataRateFeature.MinDataRate` 設定為 `null` (即使是針對 HTTP/2 要求)，以個別要求基礎來「完全停用」\*\* 讀取素率限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-202">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="a545b-203">嘗試讀取 `IHttpMinRequestBodyDataRateFeature.MinDataRate` 或嘗試將它設定為 `null` 以外的值將會導致擲回 `NotSupportedException` (假設要求是 HTTP/2 要求)。</span><span class="sxs-lookup"><span data-stu-id="a545b-203">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="a545b-204">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="a545b-204">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="a545b-205">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="a545b-205">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="a545b-206">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-206">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="a545b-207">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="a545b-207">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="a545b-208">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="a545b-208">Maximum streams per connection</span></span>

<span data-ttu-id="a545b-209">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="a545b-209">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="a545b-210">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="a545b-210">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="a545b-211">預設值是 100。</span><span class="sxs-lookup"><span data-stu-id="a545b-211">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="a545b-212">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="a545b-212">Header table size</span></span>

<span data-ttu-id="a545b-213">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="a545b-213">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="a545b-214">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="a545b-214">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="a545b-215">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="a545b-215">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="a545b-216">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="a545b-216">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="a545b-217">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="a545b-217">Maximum frame size</span></span>

<span data-ttu-id="a545b-218">`Http2.MaxFrameSize`指出伺服器所接收或傳送之 HTTP/2 連接框架承載的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-218">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="a545b-219">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="a545b-219">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="a545b-220">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="a545b-220">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="a545b-221">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="a545b-221">Maximum request header size</span></span>

<span data-ttu-id="a545b-222">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-222">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="a545b-223">這項限制適用于其壓縮和未壓縮標記法中的名稱和值。</span><span class="sxs-lookup"><span data-stu-id="a545b-223">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="a545b-224">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="a545b-224">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="a545b-225">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="a545b-225">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="a545b-226">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="a545b-226">Initial connection window size</span></span>

<span data-ttu-id="a545b-227">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-227">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="a545b-228">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-228">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="a545b-229">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="a545b-229">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="a545b-230">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="a545b-230">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="a545b-231">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="a545b-231">Initial stream window size</span></span>

<span data-ttu-id="a545b-232">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-232">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="a545b-233">要求也皆受 `Http2.InitialConnectionWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-233">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="a545b-234">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="a545b-234">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="a545b-235">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="a545b-235">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="a545b-236">同步 I/O</span><span class="sxs-lookup"><span data-stu-id="a545b-236">Synchronous I/O</span></span>

<span data-ttu-id="a545b-237"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>控制要求和回應是否允許同步的 i/o。</span><span class="sxs-lookup"><span data-stu-id="a545b-237"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous I/O is allowed for the request and response.</span></span> <span data-ttu-id="a545b-238">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="a545b-238">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="a545b-239">大量封鎖同步 i/o 作業可能會導致執行緒集區耗盡，讓應用程式無回應。</span><span class="sxs-lookup"><span data-stu-id="a545b-239">A large number of blocking synchronous I/O operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="a545b-240">只有 `AllowSynchronousIO` 在使用不支援非同步 i/o 的程式庫時才啟用。</span><span class="sxs-lookup"><span data-stu-id="a545b-240">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous I/O.</span></span>

<span data-ttu-id="a545b-241">下列範例會啟用同步 i/o：</span><span class="sxs-lookup"><span data-stu-id="a545b-241">The following example enables synchronous I/O:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="a545b-242">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="a545b-242">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="a545b-243">端點組態</span><span class="sxs-lookup"><span data-stu-id="a545b-243">Endpoint configuration</span></span>

<span data-ttu-id="a545b-244">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="a545b-244">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="a545b-245">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="a545b-245">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="a545b-246">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="a545b-246">Specify URLs using the:</span></span>

* <span data-ttu-id="a545b-247">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="a545b-247">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="a545b-248">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="a545b-248">`--urls` command-line argument.</span></span>
* <span data-ttu-id="a545b-249">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="a545b-249">`urls` host configuration key.</span></span>
* <span data-ttu-id="a545b-250">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="a545b-250">`UseUrls` extension method.</span></span>

<span data-ttu-id="a545b-251">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="a545b-251">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="a545b-252">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000;http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="a545b-252">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="a545b-253">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="a545b-253">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="a545b-254">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="a545b-254">A development certificate is created:</span></span>

* <span data-ttu-id="a545b-255">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="a545b-255">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="a545b-256">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-256">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="a545b-257">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-257">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="a545b-258">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="a545b-258">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="a545b-259">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-259">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="a545b-260">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="a545b-260">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="a545b-261">`KestrelServerOptions`配置</span><span class="sxs-lookup"><span data-stu-id="a545b-261">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="a545b-262">ConfigureEndpointDefaults （動作 \<ListenOptions> ）</span><span class="sxs-lookup"><span data-stu-id="a545b-262">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="a545b-263">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-263">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="a545b-264">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="a545b-264">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="a545b-265"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="a545b-265">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="a545b-266">ConfigureHttpsDefaults （動作 \<HttpsConnectionAdapterOptions> ）</span><span class="sxs-lookup"><span data-stu-id="a545b-266">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="a545b-267">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-267">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="a545b-268">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="a545b-268">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="a545b-269"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="a545b-269">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="a545b-270">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="a545b-270">Configure(IConfiguration)</span></span>

<span data-ttu-id="a545b-271">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-271">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="a545b-272">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="a545b-272">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="a545b-273">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="a545b-273">ListenOptions.UseHttps</span></span>

<span data-ttu-id="a545b-274">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-274">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="a545b-275">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="a545b-275">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="a545b-276">`UseHttps`：將 Kestrel 設定為使用 HTTPS 搭配預設憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-276">`UseHttps`: Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="a545b-277">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a545b-277">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="a545b-278">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="a545b-278">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="a545b-279">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="a545b-279">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="a545b-280">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="a545b-280">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="a545b-281">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="a545b-281">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="a545b-282">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="a545b-282">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="a545b-283">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="a545b-283">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="a545b-284">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="a545b-284">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="a545b-285">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-285">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="a545b-286">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="a545b-286">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="a545b-287">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-287">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="a545b-288">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-288">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="a545b-289">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-289">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="a545b-290">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="a545b-290">Supported configurations described next:</span></span>

* <span data-ttu-id="a545b-291">無組態</span><span class="sxs-lookup"><span data-stu-id="a545b-291">No configuration</span></span>
* <span data-ttu-id="a545b-292">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="a545b-292">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="a545b-293">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="a545b-293">Change the defaults in code</span></span>

<span data-ttu-id="a545b-294">*無組態*</span><span class="sxs-lookup"><span data-stu-id="a545b-294">*No configuration*</span></span>

<span data-ttu-id="a545b-295">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="a545b-295">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="a545b-296">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="a545b-296">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="a545b-297">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-297">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="a545b-298">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="a545b-298">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="a545b-299">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="a545b-299">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="a545b-300">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="a545b-300">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="a545b-301">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="a545b-301">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="a545b-302">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證]**[預設]** > \*\*\*\* 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-302">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="a545b-303">除了針對任何憑證節點使用 [路徑]\*\*\*\* 和 [密碼]\*\*\*\*，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-303">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="a545b-304">例如，**憑證**  >  **預設**憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="a545b-304">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="a545b-305">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="a545b-305">Schema notes:</span></span>

* <span data-ttu-id="a545b-306">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="a545b-306">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="a545b-307">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="a545b-307">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="a545b-308">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="a545b-308">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="a545b-309">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="a545b-309">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="a545b-310">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="a545b-310">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="a545b-311">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="a545b-311">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="a545b-312">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="a545b-312">The `Certificate` section is optional.</span></span> <span data-ttu-id="a545b-313">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="a545b-313">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="a545b-314">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="a545b-314">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="a545b-315">`Certificate`一節同時支援**路徑** &ndash; **密碼**和**主體** &ndash; **存放區**憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-315">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="a545b-316">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="a545b-316">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="a545b-317">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="a545b-317">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="a545b-318">`KestrelServerOptions.ConfigurationLoader`可以直接存取，以繼續逐一查看現有的載入器，例如所提供的載入器 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 。</span><span class="sxs-lookup"><span data-stu-id="a545b-318">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="a545b-319">您可以在方法的選項中取得每個端點的設定區段， `Endpoint` 以便讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-319">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="a545b-320">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="a545b-320">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="a545b-321">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="a545b-321">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="a545b-322">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="a545b-322">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="a545b-323">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="a545b-323">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="a545b-324">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="a545b-324">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="a545b-325">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="a545b-325">*Change the defaults in code*</span></span>

<span data-ttu-id="a545b-326">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-326">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="a545b-327">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="a545b-327">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="a545b-328">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="a545b-328">*Kestrel support for SNI*</span></span>

<span data-ttu-id="a545b-329">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="a545b-329">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="a545b-330">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-330">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="a545b-331">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="a545b-331">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="a545b-332">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="a545b-332">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="a545b-333">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-333">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="a545b-334">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="a545b-334">SNI support requires:</span></span>

* <span data-ttu-id="a545b-335">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-335">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="a545b-336">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律為 `null` 。</span><span class="sxs-lookup"><span data-stu-id="a545b-336">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="a545b-337">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="a545b-337">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="a545b-338">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-338">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="a545b-339">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-339">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="a545b-340">連接記錄</span><span class="sxs-lookup"><span data-stu-id="a545b-340">Connection logging</span></span>

<span data-ttu-id="a545b-341">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> ，以針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="a545b-341">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="a545b-342">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="a545b-342">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="a545b-343">如果 `UseConnectionLogging` 放在之前 `UseHttps` ，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="a545b-343">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="a545b-344">如果 `UseConnectionLogging` 放在之後 `UseHttps` ，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="a545b-344">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="a545b-345">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="a545b-345">Bind to a TCP socket</span></span>

<span data-ttu-id="a545b-346"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="a545b-346">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=12-18)]

<span data-ttu-id="a545b-347">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-347">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="a545b-348">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="a545b-348">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="a545b-349">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="a545b-349">Bind to a Unix socket</span></span>

<span data-ttu-id="a545b-350">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="a545b-350">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

* <span data-ttu-id="a545b-351">在 Nginx 設定檔中，將 `server`  >  `location`  >  `proxy_pass` 專案設定為 `http://unix:/tmp/{KESTREL SOCKET}:/;` 。</span><span class="sxs-lookup"><span data-stu-id="a545b-351">In the Nginx configuration file, set the `server` > `location` > `proxy_pass` entry to `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span></span> <span data-ttu-id="a545b-352">`{KESTREL SOCKET}`這是提供給的通訊端名稱 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> （例如， `kestrel-test.sock` 在上述範例中為）。</span><span class="sxs-lookup"><span data-stu-id="a545b-352">`{KESTREL SOCKET}` is the name of the socket provided to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (for example, `kestrel-test.sock` in the preceding example).</span></span>
* <span data-ttu-id="a545b-353">請確定通訊端可由 Nginx 寫入（例如， `chmod go+w /tmp/kestrel-test.sock` ）。</span><span class="sxs-lookup"><span data-stu-id="a545b-353">Ensure that the socket is writeable by Nginx (for example, `chmod go+w /tmp/kestrel-test.sock`).</span></span>

### <a name="port-0"></a><span data-ttu-id="a545b-354">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="a545b-354">Port 0</span></span>

<span data-ttu-id="a545b-355">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-355">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="a545b-356">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="a545b-356">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="a545b-357">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="a545b-357">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="a545b-358">限制</span><span class="sxs-lookup"><span data-stu-id="a545b-358">Limitations</span></span>

<span data-ttu-id="a545b-359">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="a545b-359">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="a545b-360">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="a545b-360">`--urls` command-line argument</span></span>
* <span data-ttu-id="a545b-361">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="a545b-361">`urls` host configuration key</span></span>
* <span data-ttu-id="a545b-362">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="a545b-362">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="a545b-363">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="a545b-363">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="a545b-364">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="a545b-364">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="a545b-365">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="a545b-365">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="a545b-366">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="a545b-366">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="a545b-367">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="a545b-367">IIS endpoint configuration</span></span>

<span data-ttu-id="a545b-368">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-368">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="a545b-369">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="a545b-369">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="a545b-370">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="a545b-370">ListenOptions.Protocols</span></span>

<span data-ttu-id="a545b-371">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="a545b-371">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="a545b-372">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a545b-372">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="a545b-373">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="a545b-373">`HttpProtocols` enum value</span></span> | <span data-ttu-id="a545b-374">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="a545b-374">Connection protocol permitted</span></span> |
| ---
<span data-ttu-id="a545b-375">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-375">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-376">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-376">'Blazor'</span></span>
- <span data-ttu-id="a545b-377">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-377">'Identity'</span></span>
- <span data-ttu-id="a545b-378">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-378">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-379">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-379">'Razor'</span></span>
- <span data-ttu-id="a545b-380">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-380">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-381">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-381">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-382">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-382">'Blazor'</span></span>
- <span data-ttu-id="a545b-383">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-383">'Identity'</span></span>
- <span data-ttu-id="a545b-384">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-384">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-385">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-385">'Razor'</span></span>
- <span data-ttu-id="a545b-386">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-386">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-387">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-387">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-388">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-388">'Blazor'</span></span>
- <span data-ttu-id="a545b-389">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-389">'Identity'</span></span>
- <span data-ttu-id="a545b-390">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-390">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-391">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-391">'Razor'</span></span>
- <span data-ttu-id="a545b-392">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-392">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-393">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-393">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-394">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-394">'Blazor'</span></span>
- <span data-ttu-id="a545b-395">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-395">'Identity'</span></span>
- <span data-ttu-id="a545b-396">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-396">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-397">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-397">'Razor'</span></span>
- <span data-ttu-id="a545b-398">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-398">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-399">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-399">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-400">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-400">'Blazor'</span></span>
- <span data-ttu-id="a545b-401">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-401">'Identity'</span></span>
- <span data-ttu-id="a545b-402">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-402">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-403">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-403">'Razor'</span></span>
- <span data-ttu-id="a545b-404">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-404">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-405">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-405">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-406">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-406">'Blazor'</span></span>
- <span data-ttu-id="a545b-407">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-407">'Identity'</span></span>
- <span data-ttu-id="a545b-408">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-408">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-409">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-409">'Razor'</span></span>
- <span data-ttu-id="a545b-410">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-410">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-411">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-411">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-412">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-412">'Blazor'</span></span>
- <span data-ttu-id="a545b-413">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-413">'Identity'</span></span>
- <span data-ttu-id="a545b-414">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-414">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-415">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-415">'Razor'</span></span>
- <span data-ttu-id="a545b-416">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-416">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-417">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-417">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-418">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-418">'Blazor'</span></span>
- <span data-ttu-id="a545b-419">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-419">'Identity'</span></span>
- <span data-ttu-id="a545b-420">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-420">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-421">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-421">'Razor'</span></span>
- <span data-ttu-id="a545b-422">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-422">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-423">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-423">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-424">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-424">'Blazor'</span></span>
- <span data-ttu-id="a545b-425">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-425">'Identity'</span></span>
- <span data-ttu-id="a545b-426">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-426">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-427">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-427">'Razor'</span></span>
- <span data-ttu-id="a545b-428">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-428">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-429">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-429">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-430">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-430">'Blazor'</span></span>
- <span data-ttu-id="a545b-431">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-431">'Identity'</span></span>
- <span data-ttu-id="a545b-432">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-432">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-433">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-433">'Razor'</span></span>
- <span data-ttu-id="a545b-434">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-434">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-435">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-435">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-436">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-436">'Blazor'</span></span>
- <span data-ttu-id="a545b-437">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-437">'Identity'</span></span>
- <span data-ttu-id="a545b-438">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-438">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-439">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-439">'Razor'</span></span>
- <span data-ttu-id="a545b-440">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-440">'SignalR' uid:</span></span> 

<span data-ttu-id="a545b-441">------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-441">------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-442">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-442">'Blazor'</span></span>
- <span data-ttu-id="a545b-443">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-443">'Identity'</span></span>
- <span data-ttu-id="a545b-444">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-444">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-445">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-445">'Razor'</span></span>
- <span data-ttu-id="a545b-446">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-446">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-447">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-447">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-448">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-448">'Blazor'</span></span>
- <span data-ttu-id="a545b-449">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-449">'Identity'</span></span>
- <span data-ttu-id="a545b-450">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-450">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-451">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-451">'Razor'</span></span>
- <span data-ttu-id="a545b-452">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-452">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-453">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-453">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-454">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-454">'Blazor'</span></span>
- <span data-ttu-id="a545b-455">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-455">'Identity'</span></span>
- <span data-ttu-id="a545b-456">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-456">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-457">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-457">'Razor'</span></span>
- <span data-ttu-id="a545b-458">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-458">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-459">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-459">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-460">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-460">'Blazor'</span></span>
- <span data-ttu-id="a545b-461">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-461">'Identity'</span></span>
- <span data-ttu-id="a545b-462">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-462">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-463">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-463">'Razor'</span></span>
- <span data-ttu-id="a545b-464">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-464">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-465">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-465">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-466">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-466">'Blazor'</span></span>
- <span data-ttu-id="a545b-467">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-467">'Identity'</span></span>
- <span data-ttu-id="a545b-468">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-468">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-469">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-469">'Razor'</span></span>
- <span data-ttu-id="a545b-470">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-470">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-471">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-471">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-472">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-472">'Blazor'</span></span>
- <span data-ttu-id="a545b-473">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-473">'Identity'</span></span>
- <span data-ttu-id="a545b-474">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-474">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-475">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-475">'Razor'</span></span>
- <span data-ttu-id="a545b-476">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-476">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-477">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-477">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-478">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-478">'Blazor'</span></span>
- <span data-ttu-id="a545b-479">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-479">'Identity'</span></span>
- <span data-ttu-id="a545b-480">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-480">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-481">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-481">'Razor'</span></span>
- <span data-ttu-id="a545b-482">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-482">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-483">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-483">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-484">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-484">'Blazor'</span></span>
- <span data-ttu-id="a545b-485">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-485">'Identity'</span></span>
- <span data-ttu-id="a545b-486">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-486">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-487">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-487">'Razor'</span></span>
- <span data-ttu-id="a545b-488">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-488">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-489">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-489">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-490">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-490">'Blazor'</span></span>
- <span data-ttu-id="a545b-491">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-491">'Identity'</span></span>
- <span data-ttu-id="a545b-492">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-492">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-493">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-493">'Razor'</span></span>
- <span data-ttu-id="a545b-494">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-494">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-495">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-495">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-496">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-496">'Blazor'</span></span>
- <span data-ttu-id="a545b-497">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-497">'Identity'</span></span>
- <span data-ttu-id="a545b-498">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-498">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-499">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-499">'Razor'</span></span>
- <span data-ttu-id="a545b-500">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-500">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-501">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-501">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-502">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-502">'Blazor'</span></span>
- <span data-ttu-id="a545b-503">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-503">'Identity'</span></span>
- <span data-ttu-id="a545b-504">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-504">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-505">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-505">'Razor'</span></span>
- <span data-ttu-id="a545b-506">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-506">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-507">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-507">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-508">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-508">'Blazor'</span></span>
- <span data-ttu-id="a545b-509">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-509">'Identity'</span></span>
- <span data-ttu-id="a545b-510">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-510">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-511">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-511">'Razor'</span></span>
- <span data-ttu-id="a545b-512">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-512">'SignalR' uid:</span></span> 

<span data-ttu-id="a545b-513">--------------- | |`Http1`                    |僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a545b-513">--------------- | | `Http1`                    | HTTP/1.1 only.</span></span> <span data-ttu-id="a545b-514">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="a545b-514">Can be used with or without TLS.</span></span> <span data-ttu-id="a545b-515">| |`Http2`                    |僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a545b-515">| | `Http2`                    | HTTP/2 only.</span></span> <span data-ttu-id="a545b-516">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="a545b-516">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> <span data-ttu-id="a545b-517">| |`Http1AndHttp2`            |HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a545b-517">| | `Http1AndHttp2`            | HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="a545b-518">HTTP/2 要求用戶端選取 TLS[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)交握中的 HTTP/2;否則，連接預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a545b-518">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="a545b-519">`ListenOptions.Protocols`任何端點的預設值為 `HttpProtocols.Http1AndHttp2` 。</span><span class="sxs-lookup"><span data-stu-id="a545b-519">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="a545b-520">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="a545b-520">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="a545b-521">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="a545b-521">TLS version 1.2 or later</span></span>
* <span data-ttu-id="a545b-522">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="a545b-522">Renegotiation disabled</span></span>
* <span data-ttu-id="a545b-523">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="a545b-523">Compression disabled</span></span>
* <span data-ttu-id="a545b-524">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="a545b-524">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="a545b-525">橢圓曲線 Diffie-hellman （ECDHE） &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; ：224位最小值</span><span class="sxs-lookup"><span data-stu-id="a545b-525">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;: 224 bits minimum</span></span>
  * <span data-ttu-id="a545b-526">有限欄位 diffie-hellman （DHE） &lbrack; `TLS12` &rbrack; ：2048位最小值</span><span class="sxs-lookup"><span data-stu-id="a545b-526">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;: 2048 bits minimum</span></span>
* <span data-ttu-id="a545b-527">加密套件未列於封鎖清單中</span><span class="sxs-lookup"><span data-stu-id="a545b-527">Cipher suite not blacklisted</span></span>

<span data-ttu-id="a545b-528">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`&lbrack;`TLS-ECDHE`&rbrack;根據預設，支援 P-256 橢圓曲線 &lbrack; `FIPS186` &rbrack; 。</span><span class="sxs-lookup"><span data-stu-id="a545b-528">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="a545b-529">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="a545b-529">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="a545b-530">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="a545b-530">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="a545b-531">如有需要，請使用連線中介軟體，針對特定的加密依據每個連線來篩選 TLS 交握。</span><span class="sxs-lookup"><span data-stu-id="a545b-531">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="a545b-532">下列範例 <xref:System.NotSupportedException> 會針對應用程式不支援的任何加密演算法擲回。</span><span class="sxs-lookup"><span data-stu-id="a545b-532">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="a545b-533">或者，定義 ITlsHandshakeFeature，並將[CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm)與可接受的加密套件清單進行比較。</span><span class="sxs-lookup"><span data-stu-id="a545b-533">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="a545b-534">不會使用加密來搭配[CipherAlgorithmType。 Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher 演算法。</span><span class="sxs-lookup"><span data-stu-id="a545b-534">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

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

<span data-ttu-id="a545b-535">連接篩選也可以透過 <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda 設定：</span><span class="sxs-lookup"><span data-stu-id="a545b-535">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

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

<span data-ttu-id="a545b-536">在 Linux 上， <xref:System.Net.Security.CipherSuitesPolicy> 可以用來篩選每個連線的 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="a545b-536">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

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

<span data-ttu-id="a545b-537">從設定進行通訊協定設定\*\*</span><span class="sxs-lookup"><span data-stu-id="a545b-537">*Set the protocol from configuration*</span></span>

<span data-ttu-id="a545b-538">`CreateDefaultBuilder` 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-538">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="a545b-539">下列*appsettings*範例會建立 HTTP/1.1 作為所有端點的預設連接通訊協定：</span><span class="sxs-lookup"><span data-stu-id="a545b-539">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="a545b-540">下列*appsettings*範例會建立特定端點的 HTTP/1.1 連接通訊協定：</span><span class="sxs-lookup"><span data-stu-id="a545b-540">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="a545b-541">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="a545b-541">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="a545b-542">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="a545b-542">Transport configuration</span></span>

<span data-ttu-id="a545b-543">針對需要使用 Libuv （）的專案 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> ：</span><span class="sxs-lookup"><span data-stu-id="a545b-543">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="a545b-544">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="a545b-544">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="a545b-545"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>在上呼叫 `IWebHostBuilder` ：</span><span class="sxs-lookup"><span data-stu-id="a545b-545">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="a545b-546">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="a545b-546">URL prefixes</span></span>

<span data-ttu-id="a545b-547">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="a545b-547">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="a545b-548">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="a545b-548">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="a545b-549">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-549">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="a545b-550">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-550">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="a545b-551">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="a545b-551">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="a545b-552">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-552">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="a545b-553">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="a545b-553">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="a545b-554">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-554">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="a545b-555">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="a545b-555">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="a545b-556">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="a545b-556">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="a545b-557">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="a545b-557">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="a545b-558">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="a545b-558">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="a545b-559">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-559">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="a545b-560">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="a545b-560">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="a545b-561">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="a545b-561">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="a545b-562">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="a545b-562">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="a545b-563">主機篩選</span><span class="sxs-lookup"><span data-stu-id="a545b-563">Host filtering</span></span>

<span data-ttu-id="a545b-564">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a545b-564">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="a545b-565">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="a545b-565">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="a545b-566">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="a545b-566">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="a545b-567">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="a545b-567">`Host` headers aren't validated.</span></span>

<span data-ttu-id="a545b-568">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a545b-568">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="a545b-569">主機篩選中介軟體是由[AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件提供，這是針對 ASP.NET Core 應用程式而隱含提供的。</span><span class="sxs-lookup"><span data-stu-id="a545b-569">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="a545b-570">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="a545b-570">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="a545b-571">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a545b-571">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="a545b-572">若要啟用中介軟體，請 `AllowedHosts` 在*appsettings*中定義金鑰 / *appsettings。 \<EnvironmentName>json*。</span><span class="sxs-lookup"><span data-stu-id="a545b-572">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="a545b-573">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="a545b-573">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="a545b-574">*appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="a545b-574">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="a545b-575">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="a545b-575">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="a545b-576">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="a545b-576">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="a545b-577">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="a545b-577">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="a545b-578">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="a545b-578">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="a545b-579">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="a545b-579">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="a545b-580">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="a545b-580">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="a545b-581">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="a545b-581">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="a545b-582">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="a545b-582">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="a545b-583">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a545b-583">HTTPS</span></span>
* <span data-ttu-id="a545b-584">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="a545b-584">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="a545b-585">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="a545b-585">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="a545b-586">HTTP/2 (macOS 上除外&dagger;)</span><span class="sxs-lookup"><span data-stu-id="a545b-586">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="a545b-587">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a545b-587">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="a545b-588">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-588">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="a545b-589">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a545b-589">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="a545b-590">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="a545b-590">HTTP/2 support</span></span>

<span data-ttu-id="a545b-591">如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="a545b-591">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="a545b-592">作業系統&dagger;</span><span class="sxs-lookup"><span data-stu-id="a545b-592">Operating system&dagger;</span></span>
  * <span data-ttu-id="a545b-593">Windows Server 2016/Windows 10 或更新版本&Dagger;</span><span class="sxs-lookup"><span data-stu-id="a545b-593">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="a545b-594">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="a545b-594">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="a545b-595">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a545b-595">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="a545b-596">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線</span><span class="sxs-lookup"><span data-stu-id="a545b-596">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="a545b-597">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="a545b-597">TLS 1.2 or later connection</span></span>

<span data-ttu-id="a545b-598">&dagger;未來版本的 macOS 上將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a545b-598">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="a545b-599">&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="a545b-599">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="a545b-600">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="a545b-600">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="a545b-601">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="a545b-601">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="a545b-602">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="a545b-602">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="a545b-603">預設會停用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a545b-603">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="a545b-604">如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。</span><span class="sxs-lookup"><span data-stu-id="a545b-604">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="a545b-605">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="a545b-605">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="a545b-606">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」\*\* 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="a545b-606">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="a545b-607">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-607">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="a545b-608">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="a545b-608">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="a545b-610">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="a545b-610">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a545b-612">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-612">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="a545b-613">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-613">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="a545b-614">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="a545b-614">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="a545b-615">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-615">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="a545b-616">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="a545b-616">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="a545b-617">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="a545b-617">A reverse proxy:</span></span>

* <span data-ttu-id="a545b-618">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="a545b-618">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="a545b-619">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="a545b-619">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="a545b-620">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="a545b-620">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="a545b-621">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="a545b-621">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="a545b-622">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="a545b-622">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="a545b-623">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="a545b-623">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="a545b-624">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="a545b-624">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="a545b-625">[Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)套件包含在 AspNetCore 中。[應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="a545b-625">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a545b-626">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-626">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="a545b-627">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="a545b-627">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="a545b-628">如需有關 `CreateDefaultBuilder` 和建立主機的詳細資訊，請參閱的*設定主機*一節 <xref:fundamentals/host/web-host#set-up-a-host> 。</span><span class="sxs-lookup"><span data-stu-id="a545b-628">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="a545b-629">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請使用 `ConfigureKestrel`：</span><span class="sxs-lookup"><span data-stu-id="a545b-629">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="a545b-630">如果應用程式未呼叫 `CreateDefaultBuilder` 來設定主機，請在呼叫 `ConfigureKestrel` **之前**，先呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="a545b-630">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="a545b-631">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="a545b-631">Kestrel options</span></span>

<span data-ttu-id="a545b-632">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="a545b-632">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="a545b-633">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="a545b-633">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="a545b-634">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="a545b-634">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="a545b-635">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="a545b-635">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="a545b-636">在下列範例中，以 c # 程式碼設定的 Kestrel 選項也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-636">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="a545b-637">例如，檔案設定提供者可以從*appsettings*或 Appsettings 載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="a545b-637">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="a545b-638">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="a545b-638">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="a545b-639">在中設定 Kestrel `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="a545b-639">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="a545b-640">將的實例插入 `IConfiguration` 至 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="a545b-640">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="a545b-641">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a545b-641">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="a545b-642">在中 `Startup.ConfigureServices` ，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="a545b-642">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

* <span data-ttu-id="a545b-643">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="a545b-643">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="a545b-644">在*Program.cs*中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="a545b-644">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="a545b-645">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="a545b-645">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="a545b-646">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="a545b-646">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="a545b-647">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="a545b-647">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="a545b-648">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="a545b-648">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="a545b-649">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="a545b-649">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="a545b-650">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="a545b-650">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="a545b-651">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-651">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="a545b-652">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-652">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="a545b-653">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="a545b-653">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="a545b-654">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="a545b-654">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="a545b-655">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="a545b-655">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="a545b-656">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="a545b-656">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="a545b-657">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="a545b-657">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="a545b-658">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="a545b-658">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="a545b-659">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="a545b-659">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="a545b-660">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="a545b-660">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="a545b-661">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-661">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="a545b-662">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="a545b-662">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="a545b-663">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="a545b-663">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="a545b-664">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="a545b-664">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="a545b-665">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="a545b-665">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="a545b-666">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="a545b-666">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="a545b-667">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="a545b-667">A minimum rate also applies to the response.</span></span> <span data-ttu-id="a545b-668">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="a545b-668">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="a545b-669">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="a545b-669">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="a545b-670">覆寫中介軟體中每個要求的最小速率限制：</span><span class="sxs-lookup"><span data-stu-id="a545b-670">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="a545b-671">因為通訊協定對要求多工的支援，所以 HTTP/2 不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的所有速率功能都不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。</span><span class="sxs-lookup"><span data-stu-id="a545b-671">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="a545b-672">透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="a545b-672">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="a545b-673">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="a545b-673">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="a545b-674">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-674">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="a545b-675">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="a545b-675">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="a545b-676">每個連線的資料流數目上限</span><span class="sxs-lookup"><span data-stu-id="a545b-676">Maximum streams per connection</span></span>

<span data-ttu-id="a545b-677">`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。</span><span class="sxs-lookup"><span data-stu-id="a545b-677">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="a545b-678">超出的資料流會被拒絕。</span><span class="sxs-lookup"><span data-stu-id="a545b-678">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="a545b-679">預設值是 100。</span><span class="sxs-lookup"><span data-stu-id="a545b-679">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="a545b-680">標頭表格大小</span><span class="sxs-lookup"><span data-stu-id="a545b-680">Header table size</span></span>

<span data-ttu-id="a545b-681">HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="a545b-681">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="a545b-682">`Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。</span><span class="sxs-lookup"><span data-stu-id="a545b-682">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="a545b-683">這個值是以八位元提供，而且必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="a545b-683">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="a545b-684">預設值為 4096。</span><span class="sxs-lookup"><span data-stu-id="a545b-684">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="a545b-685">框架大小上限</span><span class="sxs-lookup"><span data-stu-id="a545b-685">Maximum frame size</span></span>

<span data-ttu-id="a545b-686">`Http2.MaxFrameSize` 表示要接收的 HTTP/2 連線框架承載大小上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-686">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="a545b-687">這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。</span><span class="sxs-lookup"><span data-stu-id="a545b-687">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="a545b-688">預設值為 2^14 (16,384)。</span><span class="sxs-lookup"><span data-stu-id="a545b-688">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="a545b-689">要求標頭大小上限</span><span class="sxs-lookup"><span data-stu-id="a545b-689">Maximum request header size</span></span>

<span data-ttu-id="a545b-690">`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-690">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="a545b-691">此限制皆共同套用至已壓縮及未壓縮代表中的名稱與值。</span><span class="sxs-lookup"><span data-stu-id="a545b-691">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="a545b-692">此值必須大於零 (0)。</span><span class="sxs-lookup"><span data-stu-id="a545b-692">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="a545b-693">預設值為 8,192。</span><span class="sxs-lookup"><span data-stu-id="a545b-693">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="a545b-694">初始連線視窗大小</span><span class="sxs-lookup"><span data-stu-id="a545b-694">Initial connection window size</span></span>

<span data-ttu-id="a545b-695">`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-695">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="a545b-696">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-696">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="a545b-697">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="a545b-697">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="a545b-698">預設值為 128 KB (131,072)。</span><span class="sxs-lookup"><span data-stu-id="a545b-698">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="a545b-699">初始資料流視窗大小</span><span class="sxs-lookup"><span data-stu-id="a545b-699">Initial stream window size</span></span>

<span data-ttu-id="a545b-700">`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-700">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="a545b-701">要求也皆受 `Http2.InitialStreamWindowSize` 所限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-701">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="a545b-702">此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。</span><span class="sxs-lookup"><span data-stu-id="a545b-702">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="a545b-703">預設值為 96 KB (98,304)。</span><span class="sxs-lookup"><span data-stu-id="a545b-703">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="a545b-704">同步 I/O</span><span class="sxs-lookup"><span data-stu-id="a545b-704">Synchronous I/O</span></span>

<span data-ttu-id="a545b-705"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>控制要求和回應是否允許同步的 i/o。</span><span class="sxs-lookup"><span data-stu-id="a545b-705"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous I/O is allowed for the request and response.</span></span> <span data-ttu-id="a545b-706">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="a545b-706">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="a545b-707">大量封鎖同步 i/o 作業可能會導致執行緒集區耗盡，讓應用程式無回應。</span><span class="sxs-lookup"><span data-stu-id="a545b-707">A large number of blocking synchronous I/O operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="a545b-708">只有 `AllowSynchronousIO` 在使用不支援非同步 i/o 的程式庫時才啟用。</span><span class="sxs-lookup"><span data-stu-id="a545b-708">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous I/O.</span></span>

<span data-ttu-id="a545b-709">下列範例會啟用同步 i/o：</span><span class="sxs-lookup"><span data-stu-id="a545b-709">The following example enables synchronous I/O:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="a545b-710">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="a545b-710">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="a545b-711">端點組態</span><span class="sxs-lookup"><span data-stu-id="a545b-711">Endpoint configuration</span></span>

<span data-ttu-id="a545b-712">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="a545b-712">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="a545b-713">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="a545b-713">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="a545b-714">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="a545b-714">Specify URLs using the:</span></span>

* <span data-ttu-id="a545b-715">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="a545b-715">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="a545b-716">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="a545b-716">`--urls` command-line argument.</span></span>
* <span data-ttu-id="a545b-717">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="a545b-717">`urls` host configuration key.</span></span>
* <span data-ttu-id="a545b-718">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="a545b-718">`UseUrls` extension method.</span></span>

<span data-ttu-id="a545b-719">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="a545b-719">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="a545b-720">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000;http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="a545b-720">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="a545b-721">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="a545b-721">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="a545b-722">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="a545b-722">A development certificate is created:</span></span>

* <span data-ttu-id="a545b-723">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="a545b-723">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="a545b-724">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-724">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="a545b-725">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-725">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="a545b-726">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="a545b-726">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="a545b-727">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-727">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="a545b-728">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="a545b-728">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="a545b-729">`KestrelServerOptions`配置</span><span class="sxs-lookup"><span data-stu-id="a545b-729">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="a545b-730">ConfigureEndpointDefaults （動作 \<ListenOptions> ）</span><span class="sxs-lookup"><span data-stu-id="a545b-730">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="a545b-731">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-731">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="a545b-732">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="a545b-732">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="a545b-733"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="a545b-733">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="a545b-734">ConfigureHttpsDefaults （動作 \<HttpsConnectionAdapterOptions> ）</span><span class="sxs-lookup"><span data-stu-id="a545b-734">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="a545b-735">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-735">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="a545b-736">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="a545b-736">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="a545b-737"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="a545b-737">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>


### <a name="configureiconfiguration"></a><span data-ttu-id="a545b-738">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="a545b-738">Configure(IConfiguration)</span></span>

<span data-ttu-id="a545b-739">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-739">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="a545b-740">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="a545b-740">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="a545b-741">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="a545b-741">ListenOptions.UseHttps</span></span>

<span data-ttu-id="a545b-742">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-742">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="a545b-743">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="a545b-743">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="a545b-744">`UseHttps`：將 Kestrel 設定為使用 HTTPS 搭配預設憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-744">`UseHttps`: Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="a545b-745">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a545b-745">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="a545b-746">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="a545b-746">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="a545b-747">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="a545b-747">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="a545b-748">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="a545b-748">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="a545b-749">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="a545b-749">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="a545b-750">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="a545b-750">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="a545b-751">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="a545b-751">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="a545b-752">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="a545b-752">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="a545b-753">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-753">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="a545b-754">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="a545b-754">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="a545b-755">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-755">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="a545b-756">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-756">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="a545b-757">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-757">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="a545b-758">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="a545b-758">Supported configurations described next:</span></span>

* <span data-ttu-id="a545b-759">無組態</span><span class="sxs-lookup"><span data-stu-id="a545b-759">No configuration</span></span>
* <span data-ttu-id="a545b-760">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="a545b-760">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="a545b-761">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="a545b-761">Change the defaults in code</span></span>

<span data-ttu-id="a545b-762">*無組態*</span><span class="sxs-lookup"><span data-stu-id="a545b-762">*No configuration*</span></span>

<span data-ttu-id="a545b-763">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="a545b-763">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="a545b-764">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="a545b-764">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="a545b-765">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-765">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="a545b-766">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="a545b-766">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="a545b-767">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="a545b-767">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="a545b-768">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="a545b-768">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="a545b-769">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="a545b-769">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="a545b-770">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證]**[預設]** > \*\*\*\* 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-770">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="a545b-771">除了針對任何憑證節點使用 [路徑]\*\*\*\* 和 [密碼]\*\*\*\*，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-771">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="a545b-772">例如，**憑證**  >  **預設**憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="a545b-772">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="a545b-773">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="a545b-773">Schema notes:</span></span>

* <span data-ttu-id="a545b-774">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="a545b-774">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="a545b-775">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="a545b-775">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="a545b-776">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="a545b-776">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="a545b-777">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="a545b-777">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="a545b-778">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="a545b-778">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="a545b-779">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="a545b-779">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="a545b-780">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="a545b-780">The `Certificate` section is optional.</span></span> <span data-ttu-id="a545b-781">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="a545b-781">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="a545b-782">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="a545b-782">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="a545b-783">`Certificate`一節同時支援**路徑** &ndash; **密碼**和**主體** &ndash; **存放區**憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-783">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="a545b-784">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="a545b-784">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="a545b-785">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="a545b-785">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="a545b-786">`KestrelServerOptions.ConfigurationLoader`可以直接存取，以繼續逐一查看現有的載入器，例如所提供的載入器 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 。</span><span class="sxs-lookup"><span data-stu-id="a545b-786">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="a545b-787">您可以在方法的選項中取得每個端點的設定區段， `Endpoint` 以便讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-787">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="a545b-788">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="a545b-788">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="a545b-789">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="a545b-789">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="a545b-790">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="a545b-790">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="a545b-791">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="a545b-791">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="a545b-792">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="a545b-792">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="a545b-793">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="a545b-793">*Change the defaults in code*</span></span>

<span data-ttu-id="a545b-794">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-794">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="a545b-795">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="a545b-795">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="a545b-796">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="a545b-796">*Kestrel support for SNI*</span></span>

<span data-ttu-id="a545b-797">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="a545b-797">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="a545b-798">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-798">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="a545b-799">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="a545b-799">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="a545b-800">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="a545b-800">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="a545b-801">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-801">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="a545b-802">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="a545b-802">SNI support requires:</span></span>

* <span data-ttu-id="a545b-803">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-803">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="a545b-804">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律為 `null` 。</span><span class="sxs-lookup"><span data-stu-id="a545b-804">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="a545b-805">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="a545b-805">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="a545b-806">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-806">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="a545b-807">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-807">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="a545b-808">連接記錄</span><span class="sxs-lookup"><span data-stu-id="a545b-808">Connection logging</span></span>

<span data-ttu-id="a545b-809">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> ，以針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="a545b-809">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="a545b-810">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="a545b-810">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="a545b-811">如果 `UseConnectionLogging` 放在之前 `UseHttps` ，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="a545b-811">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="a545b-812">如果 `UseConnectionLogging` 放在之後 `UseHttps` ，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="a545b-812">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="a545b-813">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="a545b-813">Bind to a TCP socket</span></span>

<span data-ttu-id="a545b-814"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="a545b-814">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="a545b-815">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-815">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="a545b-816">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="a545b-816">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="a545b-817">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="a545b-817">Bind to a Unix socket</span></span>

<span data-ttu-id="a545b-818">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="a545b-818">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

* <span data-ttu-id="a545b-819">在 Nginx confiuguration 檔中，將 `server`  >  `location`  >  `proxy_pass` 專案設定為 `http://unix:/tmp/{KESTREL SOCKET}:/;` 。</span><span class="sxs-lookup"><span data-stu-id="a545b-819">In the Nginx confiuguration file, set the `server` > `location` > `proxy_pass` entry to `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span></span> <span data-ttu-id="a545b-820">`{KESTREL SOCKET}`這是提供給的通訊端名稱 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> （例如， `kestrel-test.sock` 在上述範例中為）。</span><span class="sxs-lookup"><span data-stu-id="a545b-820">`{KESTREL SOCKET}` is the name of the socket provided to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (for example, `kestrel-test.sock` in the preceding example).</span></span>
* <span data-ttu-id="a545b-821">請確定通訊端可由 Nginx 寫入（例如， `chmod go+w /tmp/kestrel-test.sock` ）。</span><span class="sxs-lookup"><span data-stu-id="a545b-821">Ensure that the socket is writeable by Nginx (for example, `chmod go+w /tmp/kestrel-test.sock`).</span></span> 

### <a name="port-0"></a><span data-ttu-id="a545b-822">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="a545b-822">Port 0</span></span>

<span data-ttu-id="a545b-823">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-823">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="a545b-824">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="a545b-824">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="a545b-825">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="a545b-825">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="a545b-826">限制</span><span class="sxs-lookup"><span data-stu-id="a545b-826">Limitations</span></span>

<span data-ttu-id="a545b-827">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="a545b-827">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="a545b-828">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="a545b-828">`--urls` command-line argument</span></span>
* <span data-ttu-id="a545b-829">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="a545b-829">`urls` host configuration key</span></span>
* <span data-ttu-id="a545b-830">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="a545b-830">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="a545b-831">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="a545b-831">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="a545b-832">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="a545b-832">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="a545b-833">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="a545b-833">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="a545b-834">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="a545b-834">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="a545b-835">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="a545b-835">IIS endpoint configuration</span></span>

<span data-ttu-id="a545b-836">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-836">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="a545b-837">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="a545b-837">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="a545b-838">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="a545b-838">ListenOptions.Protocols</span></span>

<span data-ttu-id="a545b-839">`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。</span><span class="sxs-lookup"><span data-stu-id="a545b-839">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="a545b-840">從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a545b-840">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="a545b-841">`HttpProtocols` 列舉值</span><span class="sxs-lookup"><span data-stu-id="a545b-841">`HttpProtocols` enum value</span></span> | <span data-ttu-id="a545b-842">允許的連線通訊協定</span><span class="sxs-lookup"><span data-stu-id="a545b-842">Connection protocol permitted</span></span> |
| ---
<span data-ttu-id="a545b-843">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-843">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-844">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-844">'Blazor'</span></span>
- <span data-ttu-id="a545b-845">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-845">'Identity'</span></span>
- <span data-ttu-id="a545b-846">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-846">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-847">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-847">'Razor'</span></span>
- <span data-ttu-id="a545b-848">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-848">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-849">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-849">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-850">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-850">'Blazor'</span></span>
- <span data-ttu-id="a545b-851">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-851">'Identity'</span></span>
- <span data-ttu-id="a545b-852">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-852">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-853">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-853">'Razor'</span></span>
- <span data-ttu-id="a545b-854">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-854">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-855">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-855">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-856">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-856">'Blazor'</span></span>
- <span data-ttu-id="a545b-857">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-857">'Identity'</span></span>
- <span data-ttu-id="a545b-858">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-858">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-859">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-859">'Razor'</span></span>
- <span data-ttu-id="a545b-860">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-860">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-861">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-861">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-862">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-862">'Blazor'</span></span>
- <span data-ttu-id="a545b-863">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-863">'Identity'</span></span>
- <span data-ttu-id="a545b-864">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-864">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-865">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-865">'Razor'</span></span>
- <span data-ttu-id="a545b-866">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-866">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-867">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-867">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-868">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-868">'Blazor'</span></span>
- <span data-ttu-id="a545b-869">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-869">'Identity'</span></span>
- <span data-ttu-id="a545b-870">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-870">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-871">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-871">'Razor'</span></span>
- <span data-ttu-id="a545b-872">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-872">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-873">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-873">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-874">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-874">'Blazor'</span></span>
- <span data-ttu-id="a545b-875">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-875">'Identity'</span></span>
- <span data-ttu-id="a545b-876">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-876">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-877">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-877">'Razor'</span></span>
- <span data-ttu-id="a545b-878">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-878">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-879">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-879">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-880">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-880">'Blazor'</span></span>
- <span data-ttu-id="a545b-881">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-881">'Identity'</span></span>
- <span data-ttu-id="a545b-882">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-882">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-883">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-883">'Razor'</span></span>
- <span data-ttu-id="a545b-884">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-884">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-885">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-885">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-886">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-886">'Blazor'</span></span>
- <span data-ttu-id="a545b-887">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-887">'Identity'</span></span>
- <span data-ttu-id="a545b-888">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-888">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-889">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-889">'Razor'</span></span>
- <span data-ttu-id="a545b-890">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-890">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-891">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-891">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-892">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-892">'Blazor'</span></span>
- <span data-ttu-id="a545b-893">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-893">'Identity'</span></span>
- <span data-ttu-id="a545b-894">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-894">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-895">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-895">'Razor'</span></span>
- <span data-ttu-id="a545b-896">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-896">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-897">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-897">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-898">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-898">'Blazor'</span></span>
- <span data-ttu-id="a545b-899">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-899">'Identity'</span></span>
- <span data-ttu-id="a545b-900">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-900">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-901">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-901">'Razor'</span></span>
- <span data-ttu-id="a545b-902">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-902">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-903">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-903">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-904">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-904">'Blazor'</span></span>
- <span data-ttu-id="a545b-905">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-905">'Identity'</span></span>
- <span data-ttu-id="a545b-906">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-906">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-907">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-907">'Razor'</span></span>
- <span data-ttu-id="a545b-908">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-908">'SignalR' uid:</span></span> 

<span data-ttu-id="a545b-909">------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-909">------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-910">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-910">'Blazor'</span></span>
- <span data-ttu-id="a545b-911">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-911">'Identity'</span></span>
- <span data-ttu-id="a545b-912">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-912">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-913">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-913">'Razor'</span></span>
- <span data-ttu-id="a545b-914">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-914">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-915">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-915">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-916">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-916">'Blazor'</span></span>
- <span data-ttu-id="a545b-917">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-917">'Identity'</span></span>
- <span data-ttu-id="a545b-918">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-918">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-919">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-919">'Razor'</span></span>
- <span data-ttu-id="a545b-920">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-920">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-921">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-921">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-922">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-922">'Blazor'</span></span>
- <span data-ttu-id="a545b-923">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-923">'Identity'</span></span>
- <span data-ttu-id="a545b-924">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-924">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-925">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-925">'Razor'</span></span>
- <span data-ttu-id="a545b-926">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-926">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-927">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-927">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-928">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-928">'Blazor'</span></span>
- <span data-ttu-id="a545b-929">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-929">'Identity'</span></span>
- <span data-ttu-id="a545b-930">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-930">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-931">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-931">'Razor'</span></span>
- <span data-ttu-id="a545b-932">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-932">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-933">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-933">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-934">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-934">'Blazor'</span></span>
- <span data-ttu-id="a545b-935">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-935">'Identity'</span></span>
- <span data-ttu-id="a545b-936">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-936">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-937">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-937">'Razor'</span></span>
- <span data-ttu-id="a545b-938">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-938">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-939">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-939">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-940">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-940">'Blazor'</span></span>
- <span data-ttu-id="a545b-941">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-941">'Identity'</span></span>
- <span data-ttu-id="a545b-942">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-942">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-943">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-943">'Razor'</span></span>
- <span data-ttu-id="a545b-944">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-944">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-945">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-945">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-946">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-946">'Blazor'</span></span>
- <span data-ttu-id="a545b-947">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-947">'Identity'</span></span>
- <span data-ttu-id="a545b-948">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-948">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-949">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-949">'Razor'</span></span>
- <span data-ttu-id="a545b-950">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-950">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-951">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-951">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-952">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-952">'Blazor'</span></span>
- <span data-ttu-id="a545b-953">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-953">'Identity'</span></span>
- <span data-ttu-id="a545b-954">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-954">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-955">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-955">'Razor'</span></span>
- <span data-ttu-id="a545b-956">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-956">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-957">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-957">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-958">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-958">'Blazor'</span></span>
- <span data-ttu-id="a545b-959">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-959">'Identity'</span></span>
- <span data-ttu-id="a545b-960">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-960">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-961">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-961">'Razor'</span></span>
- <span data-ttu-id="a545b-962">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-962">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-963">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-963">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-964">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-964">'Blazor'</span></span>
- <span data-ttu-id="a545b-965">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-965">'Identity'</span></span>
- <span data-ttu-id="a545b-966">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-966">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-967">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-967">'Razor'</span></span>
- <span data-ttu-id="a545b-968">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-968">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-969">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-969">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-970">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-970">'Blazor'</span></span>
- <span data-ttu-id="a545b-971">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-971">'Identity'</span></span>
- <span data-ttu-id="a545b-972">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-972">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-973">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-973">'Razor'</span></span>
- <span data-ttu-id="a545b-974">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-974">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a545b-975">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a545b-975">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a545b-976">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a545b-976">'Blazor'</span></span>
- <span data-ttu-id="a545b-977">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a545b-977">'Identity'</span></span>
- <span data-ttu-id="a545b-978">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a545b-978">'Let's Encrypt'</span></span>
- <span data-ttu-id="a545b-979">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a545b-979">'Razor'</span></span>
- <span data-ttu-id="a545b-980">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a545b-980">'SignalR' uid:</span></span> 

<span data-ttu-id="a545b-981">--------------- | |`Http1`                    |僅限 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a545b-981">--------------- | | `Http1`                    | HTTP/1.1 only.</span></span> <span data-ttu-id="a545b-982">可在具有或沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="a545b-982">Can be used with or without TLS.</span></span> <span data-ttu-id="a545b-983">| |`Http2`                    |僅限 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a545b-983">| | `Http2`                    | HTTP/2 only.</span></span> <span data-ttu-id="a545b-984">只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。</span><span class="sxs-lookup"><span data-stu-id="a545b-984">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> <span data-ttu-id="a545b-985">| |`Http1AndHttp2`            |HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a545b-985">| | `Http1AndHttp2`            | HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="a545b-986">HTTP/2 需要 TLS 和[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)連線;否則，連接預設為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a545b-986">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="a545b-987">預設通訊協定為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a545b-987">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="a545b-988">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="a545b-988">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="a545b-989">TLS 1.2 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="a545b-989">TLS version 1.2 or later</span></span>
* <span data-ttu-id="a545b-990">已停用重新交涉</span><span class="sxs-lookup"><span data-stu-id="a545b-990">Renegotiation disabled</span></span>
* <span data-ttu-id="a545b-991">已停用壓縮</span><span class="sxs-lookup"><span data-stu-id="a545b-991">Compression disabled</span></span>
* <span data-ttu-id="a545b-992">暫時金鑰交換大小下限：</span><span class="sxs-lookup"><span data-stu-id="a545b-992">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="a545b-993">橢圓曲線 Diffie-hellman （ECDHE） &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; ：224位最小值</span><span class="sxs-lookup"><span data-stu-id="a545b-993">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;: 224 bits minimum</span></span>
  * <span data-ttu-id="a545b-994">有限欄位 diffie-hellman （DHE） &lbrack; `TLS12` &rbrack; ：2048位最小值</span><span class="sxs-lookup"><span data-stu-id="a545b-994">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;: 2048 bits minimum</span></span>
* <span data-ttu-id="a545b-995">加密套件未列於封鎖清單中</span><span class="sxs-lookup"><span data-stu-id="a545b-995">Cipher suite not blacklisted</span></span>

<span data-ttu-id="a545b-996">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`&lbrack;`TLS-ECDHE`&rbrack;根據預設，支援 P-256 橢圓曲線 &lbrack; `FIPS186` &rbrack; 。</span><span class="sxs-lookup"><span data-stu-id="a545b-996">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="a545b-997">下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。</span><span class="sxs-lookup"><span data-stu-id="a545b-997">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="a545b-998">這些連線使用提供的憑證受到 TLS 保護：</span><span class="sxs-lookup"><span data-stu-id="a545b-998">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="a545b-999">選擇性地建立 `IConnectionAdapter` 實作，針對每個連線來篩選特定加密的 TLS 交握：</span><span class="sxs-lookup"><span data-stu-id="a545b-999">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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

<span data-ttu-id="a545b-1000">從設定進行通訊協定設定\*\*</span><span class="sxs-lookup"><span data-stu-id="a545b-1000">*Set the protocol from configuration*</span></span>

<span data-ttu-id="a545b-1001"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-1001"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="a545b-1002">在下列 *appsettings.json* 範例中，會為 Kestrel 的所有端點建立預設連線通訊協定 (HTTP/1.1 和 HTTP/2)：</span><span class="sxs-lookup"><span data-stu-id="a545b-1002">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="a545b-1003">下列設定檔範例會為特定端點建立連線通訊協定：</span><span class="sxs-lookup"><span data-stu-id="a545b-1003">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="a545b-1004">程式碼中指定的通訊協定會覆寫設定所設定的值。</span><span class="sxs-lookup"><span data-stu-id="a545b-1004">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="a545b-1005">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="a545b-1005">Transport configuration</span></span>

<span data-ttu-id="a545b-1006">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="a545b-1006">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="a545b-1007">對於升級到 2.1 且會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> 並相依於下列任一套件的 ASP.NET Core 2.0 應用程式來說，這是一項中斷性變更：</span><span class="sxs-lookup"><span data-stu-id="a545b-1007">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="a545b-1008">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="a545b-1008">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="a545b-1009">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="a545b-1009">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="a545b-1010">針對需要使用 Libuv 的專案：</span><span class="sxs-lookup"><span data-stu-id="a545b-1010">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="a545b-1011">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="a545b-1011">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="a545b-1012">呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="a545b-1012">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="a545b-1013">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="a545b-1013">URL prefixes</span></span>

<span data-ttu-id="a545b-1014">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="a545b-1014">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="a545b-1015">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="a545b-1015">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="a545b-1016">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-1016">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="a545b-1017">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-1017">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="a545b-1018">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="a545b-1018">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="a545b-1019">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-1019">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="a545b-1020">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="a545b-1020">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="a545b-1021">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-1021">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="a545b-1022">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="a545b-1022">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="a545b-1023">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="a545b-1023">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="a545b-1024">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1024">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="a545b-1025">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1025">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="a545b-1026">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-1026">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="a545b-1027">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="a545b-1027">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="a545b-1028">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="a545b-1028">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="a545b-1029">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="a545b-1029">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="a545b-1030">主機篩選</span><span class="sxs-lookup"><span data-stu-id="a545b-1030">Host filtering</span></span>

<span data-ttu-id="a545b-1031">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a545b-1031">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="a545b-1032">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="a545b-1032">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="a545b-1033">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="a545b-1033">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="a545b-1034">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1034">`Host` headers aren't validated.</span></span>

<span data-ttu-id="a545b-1035">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a545b-1035">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="a545b-1036">主機篩選中介軟體是由[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)（ASP.NET Core 2.1 或2.2）所包含的[HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件所提供。</span><span class="sxs-lookup"><span data-stu-id="a545b-1036">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="a545b-1037">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="a545b-1037">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="a545b-1038">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a545b-1038">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="a545b-1039">若要啟用中介軟體，請 `AllowedHosts` 在*appsettings*中定義金鑰 / *appsettings。 \<EnvironmentName>json*。</span><span class="sxs-lookup"><span data-stu-id="a545b-1039">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="a545b-1040">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="a545b-1040">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="a545b-1041">*appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="a545b-1041">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="a545b-1042">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="a545b-1042">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="a545b-1043">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="a545b-1043">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="a545b-1044">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1044">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="a545b-1045">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1045">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="a545b-1046">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="a545b-1046">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="a545b-1047">Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1047">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="a545b-1048">Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="a545b-1048">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="a545b-1049">Kestrel 支援下列案例：</span><span class="sxs-lookup"><span data-stu-id="a545b-1049">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="a545b-1050">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a545b-1050">HTTPS</span></span>
* <span data-ttu-id="a545b-1051">用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級</span><span class="sxs-lookup"><span data-stu-id="a545b-1051">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="a545b-1052">Nginx 背後的高效能 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="a545b-1052">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="a545b-1053">.NET Core 支援的所有平台和版本都支援 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-1053">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="a545b-1054">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a545b-1054">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="a545b-1055">何時搭配使用 Kestrel 與反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="a545b-1055">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="a545b-1056">您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」\*\* 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="a545b-1056">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="a545b-1057">反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-1057">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="a545b-1058">Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="a545b-1058">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="a545b-1060">Kestrel 用於反向 Proxy 組態中：</span><span class="sxs-lookup"><span data-stu-id="a545b-1060">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a545b-1062">無論是否使用反向 proxy 伺服器設定，都是支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-1062">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="a545b-1063">Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-1063">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="a545b-1064">當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。</span><span class="sxs-lookup"><span data-stu-id="a545b-1064">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="a545b-1065">可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-1065">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="a545b-1066">即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="a545b-1066">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="a545b-1067">反向 Proxy：</span><span class="sxs-lookup"><span data-stu-id="a545b-1067">A reverse proxy:</span></span>

* <span data-ttu-id="a545b-1068">可以限制它所主控之應用程式的公開介面區。</span><span class="sxs-lookup"><span data-stu-id="a545b-1068">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="a545b-1069">提供額外的組態和防禦層。</span><span class="sxs-lookup"><span data-stu-id="a545b-1069">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="a545b-1070">能夠與現有基礎結構更好地整合。</span><span class="sxs-lookup"><span data-stu-id="a545b-1070">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="a545b-1071">簡化負載平衡和安全通訊 (HTTPS) 組態。</span><span class="sxs-lookup"><span data-stu-id="a545b-1071">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="a545b-1072">只有反向 proxy 伺服器需要 x.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器進行通訊。</span><span class="sxs-lookup"><span data-stu-id="a545b-1072">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="a545b-1073">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1073">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="a545b-1074">如何在 ASP.NET Core 應用程式中使用 Kestrel</span><span class="sxs-lookup"><span data-stu-id="a545b-1074">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="a545b-1075">[Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)套件包含在 AspNetCore 中。[應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="a545b-1075">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a545b-1076">ASP.NET Core 專案範本預設會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-1076">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="a545b-1077">在 *Program.cs* 中，範本程式碼會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，而後者會在幕後呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>。</span><span class="sxs-lookup"><span data-stu-id="a545b-1077">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="a545b-1078">若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>：</span><span class="sxs-lookup"><span data-stu-id="a545b-1078">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="a545b-1079">如需有關 `CreateDefaultBuilder` 和建立主機的詳細資訊，請參閱的*設定主機*一節 <xref:fundamentals/host/web-host#set-up-a-host> 。</span><span class="sxs-lookup"><span data-stu-id="a545b-1079">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="a545b-1080">Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="a545b-1080">Kestrel options</span></span>

<span data-ttu-id="a545b-1081">Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。</span><span class="sxs-lookup"><span data-stu-id="a545b-1081">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="a545b-1082">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 類別的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> 屬性上設定條件約束。</span><span class="sxs-lookup"><span data-stu-id="a545b-1082">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="a545b-1083">`Limits` 屬性會保存 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="a545b-1083">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="a545b-1084">下列範例會使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core> 命名空間；</span><span class="sxs-lookup"><span data-stu-id="a545b-1084">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="a545b-1085">在下列範例中，以 c # 程式碼設定的 Kestrel 選項也可以使用設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-1085">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="a545b-1086">例如，檔案設定提供者可以從*appsettings*或 Appsettings 載入 Kestrel 設定 *。 {環境}. json*檔案：</span><span class="sxs-lookup"><span data-stu-id="a545b-1086">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="a545b-1087">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="a545b-1087">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="a545b-1088">在中設定 Kestrel `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="a545b-1088">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="a545b-1089">將的實例插入 `IConfiguration` 至 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="a545b-1089">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="a545b-1090">下列範例假設插入的設定已指派給 `Configuration` 屬性。</span><span class="sxs-lookup"><span data-stu-id="a545b-1090">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="a545b-1091">在中 `Startup.ConfigureServices` ，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="a545b-1091">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

* <span data-ttu-id="a545b-1092">建立主機時設定 Kestrel：</span><span class="sxs-lookup"><span data-stu-id="a545b-1092">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="a545b-1093">在*Program.cs*中，將設定的 `Kestrel` 區段載入 Kestrel 的設定中：</span><span class="sxs-lookup"><span data-stu-id="a545b-1093">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="a545b-1094">上述兩種方法都適用于任何設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1094">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="a545b-1095">Keep-alive 逾時</span><span class="sxs-lookup"><span data-stu-id="a545b-1095">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="a545b-1096">取得或設定 [Keep-alive 逾時](https://tools.ietf.org/html/rfc7230#section-6.5) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1096">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="a545b-1097">預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="a545b-1097">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="a545b-1098">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="a545b-1098">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="a545b-1099">可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：</span><span class="sxs-lookup"><span data-stu-id="a545b-1099">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="a545b-1100">已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-1100">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="a545b-1101">升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-1101">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="a545b-1102">連線數目上限預設為無限制 (null)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1102">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="a545b-1103">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="a545b-1103">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="a545b-1104">預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。</span><span class="sxs-lookup"><span data-stu-id="a545b-1104">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="a545b-1105">若要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>屬性：</span><span class="sxs-lookup"><span data-stu-id="a545b-1105">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="a545b-1106">以下範例會示範如何設定應用程式、每個要求的條件約束：</span><span class="sxs-lookup"><span data-stu-id="a545b-1106">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="a545b-1107">覆寫中介軟體中特定要求的設定：</span><span class="sxs-lookup"><span data-stu-id="a545b-1107">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="a545b-1108">如果應用程式在開始讀取要求之後，設定要求的限制，就會擲回例外狀況（exception）。</span><span class="sxs-lookup"><span data-stu-id="a545b-1108">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="a545b-1109">有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。</span><span class="sxs-lookup"><span data-stu-id="a545b-1109">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="a545b-1110">當應用程式是在 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)後方於[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)執行時，Kestrel 的要求本文大小限制將會被停用，因為 IIS 已經設定限制。</span><span class="sxs-lookup"><span data-stu-id="a545b-1110">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="a545b-1111">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="a545b-1111">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="a545b-1112">如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。</span><span class="sxs-lookup"><span data-stu-id="a545b-1112">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="a545b-1113">如果速率低於最小值，則連接會超時。寬限期是指 Kestrel 提供用戶端將其傳送速率增加到最小值的時間量。在這段時間內不會檢查速率。</span><span class="sxs-lookup"><span data-stu-id="a545b-1113">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="a545b-1114">寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。</span><span class="sxs-lookup"><span data-stu-id="a545b-1114">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="a545b-1115">預設速率下限為 240 個位元組/秒，寬限期為 5 秒。</span><span class="sxs-lookup"><span data-stu-id="a545b-1115">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="a545b-1116">速率下限也適用於回應。</span><span class="sxs-lookup"><span data-stu-id="a545b-1116">A minimum rate also applies to the response.</span></span> <span data-ttu-id="a545b-1117">除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。</span><span class="sxs-lookup"><span data-stu-id="a545b-1117">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="a545b-1118">以下範例示範如何在 *Program.cs* 中設定資料速率下限：</span><span class="sxs-lookup"><span data-stu-id="a545b-1118">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="a545b-1119">要求標頭逾時</span><span class="sxs-lookup"><span data-stu-id="a545b-1119">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="a545b-1120">取得或設定伺服器花費在接收要求標頭的時間上限。</span><span class="sxs-lookup"><span data-stu-id="a545b-1120">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="a545b-1121">預設為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="a545b-1121">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="a545b-1122">同步 I/O</span><span class="sxs-lookup"><span data-stu-id="a545b-1122">Synchronous I/O</span></span>

<span data-ttu-id="a545b-1123"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>控制要求和回應是否允許同步的 i/o。</span><span class="sxs-lookup"><span data-stu-id="a545b-1123"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous I/O is allowed for the request and response.</span></span> <span data-ttu-id="a545b-1124">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1124">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="a545b-1125">大量封鎖同步 i/o 作業可能會導致執行緒集區耗盡，讓應用程式無回應。</span><span class="sxs-lookup"><span data-stu-id="a545b-1125">A large number of blocking synchronous I/O operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="a545b-1126">只有 `AllowSynchronousIO` 在使用不支援非同步 i/o 的程式庫時才啟用。</span><span class="sxs-lookup"><span data-stu-id="a545b-1126">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous I/O.</span></span>

<span data-ttu-id="a545b-1127">下列範例會停用同步 i/o：</span><span class="sxs-lookup"><span data-stu-id="a545b-1127">The following example disables synchronous I/O:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="a545b-1128">如需其他 Kestrel 選項和限制的資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="a545b-1128">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="a545b-1129">端點組態</span><span class="sxs-lookup"><span data-stu-id="a545b-1129">Endpoint configuration</span></span>

<span data-ttu-id="a545b-1130">ASP.NET Core 預設會繫結至：</span><span class="sxs-lookup"><span data-stu-id="a545b-1130">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="a545b-1131">`https://localhost:5001` (當有本機開發憑證存在時)</span><span class="sxs-lookup"><span data-stu-id="a545b-1131">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="a545b-1132">使用以下各項指定 URL：</span><span class="sxs-lookup"><span data-stu-id="a545b-1132">Specify URLs using the:</span></span>

* <span data-ttu-id="a545b-1133">`ASPNETCORE_URLS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="a545b-1133">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="a545b-1134">`--urls` 命令列引數。</span><span class="sxs-lookup"><span data-stu-id="a545b-1134">`--urls` command-line argument.</span></span>
* <span data-ttu-id="a545b-1135">`urls` 主機組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="a545b-1135">`urls` host configuration key.</span></span>
* <span data-ttu-id="a545b-1136">`UseUrls` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="a545b-1136">`UseUrls` extension method.</span></span>

<span data-ttu-id="a545b-1137">使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1137">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="a545b-1138">將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000;http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1138">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="a545b-1139">如需有關這些方法的詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1139">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="a545b-1140">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="a545b-1140">A development certificate is created:</span></span>

* <span data-ttu-id="a545b-1141">已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。</span><span class="sxs-lookup"><span data-stu-id="a545b-1141">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="a545b-1142">[dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1142">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="a545b-1143">有些瀏覽器需要授與明確的許可權，才能信任本機開發憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1143">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="a545b-1144">專案範本預設會將應用程式設定為在 HTTPS 上執行，並包含 HTTPS 重新導向[和 HSTS 支援](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1144">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="a545b-1145">請在 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 方法，來為 Kestrel 設定 URL 首碼和連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-1145">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="a545b-1146">`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1146">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="a545b-1147">`KestrelServerOptions`配置</span><span class="sxs-lookup"><span data-stu-id="a545b-1147">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="a545b-1148">ConfigureEndpointDefaults （動作 \<ListenOptions> ）</span><span class="sxs-lookup"><span data-stu-id="a545b-1148">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="a545b-1149">指定組態 `Action` 以針對每個指定端點執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-1149">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="a545b-1150">呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1150">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="a545b-1151"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="a545b-1151">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="a545b-1152">ConfigureHttpsDefaults （動作 \<HttpsConnectionAdapterOptions> ）</span><span class="sxs-lookup"><span data-stu-id="a545b-1152">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="a545b-1153">指定組態 `Action` 以針對每個 HTTPS 端點執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-1153">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="a545b-1154">呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1154">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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
> <span data-ttu-id="a545b-1155"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> 的端點，將不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="a545b-1155">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="a545b-1156">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="a545b-1156">Configure(IConfiguration)</span></span>

<span data-ttu-id="a545b-1157">建立設定載入器來設定以 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作為輸入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a545b-1157">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="a545b-1158">組態的範圍必須限於 Kestrel 的組態區段。</span><span class="sxs-lookup"><span data-stu-id="a545b-1158">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="a545b-1159">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="a545b-1159">ListenOptions.UseHttps</span></span>

<span data-ttu-id="a545b-1160">設定 Kestrel 使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-1160">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="a545b-1161">`ListenOptions.UseHttps` 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="a545b-1161">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="a545b-1162">`UseHttps`：將 Kestrel 設定為使用 HTTPS 搭配預設憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1162">`UseHttps`: Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="a545b-1163">如果未設定預設憑證，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a545b-1163">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="a545b-1164">`ListenOptions.UseHttps` 參數：</span><span class="sxs-lookup"><span data-stu-id="a545b-1164">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="a545b-1165">`filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="a545b-1165">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="a545b-1166">`password` 是存取 X.509 憑證資料所需的密碼。</span><span class="sxs-lookup"><span data-stu-id="a545b-1166">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="a545b-1167">`configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1167">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="a545b-1168">傳回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1168">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="a545b-1169">`storeName` 是要從中載入憑證的憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="a545b-1169">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="a545b-1170">`subject` 是憑證的主體名稱。</span><span class="sxs-lookup"><span data-stu-id="a545b-1170">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="a545b-1171">`allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1171">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="a545b-1172">`location` 是要從中載入憑證的存放區位置。</span><span class="sxs-lookup"><span data-stu-id="a545b-1172">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="a545b-1173">`serverCertificate` 是 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1173">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="a545b-1174">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-1174">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="a545b-1175">至少必須提供預設憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1175">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="a545b-1176">支援的組態描述如下：</span><span class="sxs-lookup"><span data-stu-id="a545b-1176">Supported configurations described next:</span></span>

* <span data-ttu-id="a545b-1177">無組態</span><span class="sxs-lookup"><span data-stu-id="a545b-1177">No configuration</span></span>
* <span data-ttu-id="a545b-1178">從組態取代預設憑證</span><span class="sxs-lookup"><span data-stu-id="a545b-1178">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="a545b-1179">變更程式碼中的預設值</span><span class="sxs-lookup"><span data-stu-id="a545b-1179">Change the defaults in code</span></span>

<span data-ttu-id="a545b-1180">*無組態*</span><span class="sxs-lookup"><span data-stu-id="a545b-1180">*No configuration*</span></span>

<span data-ttu-id="a545b-1181">Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1181">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="a545b-1182">*從組態取代預設憑證*</span><span class="sxs-lookup"><span data-stu-id="a545b-1182">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="a545b-1183">`CreateDefaultBuilder` 預設會呼叫 `Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-1183">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="a545b-1184">Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="a545b-1184">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="a545b-1185">設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="a545b-1185">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="a545b-1186">在下列 *appsettings.json* 範例中：</span><span class="sxs-lookup"><span data-stu-id="a545b-1186">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="a545b-1187">將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1187">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="a545b-1188">任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證]**[預設]** > \*\*\*\* 下定義的憑證或開發憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1188">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="a545b-1189">除了針對任何憑證節點使用 [路徑]\*\*\*\* 和 [密碼]\*\*\*\*，還可以使用憑證存放區欄位指定憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1189">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="a545b-1190">例如，**憑證**  >  **預設**憑證可以指定為：</span><span class="sxs-lookup"><span data-stu-id="a545b-1190">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="a545b-1191">結構描述附註：</span><span class="sxs-lookup"><span data-stu-id="a545b-1191">Schema notes:</span></span>

* <span data-ttu-id="a545b-1192">端點名稱不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="a545b-1192">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="a545b-1193">例如，`HTTPS` 和 `Https` 都有效。</span><span class="sxs-lookup"><span data-stu-id="a545b-1193">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="a545b-1194">`Url` 參數對每個端點而言都是必要的。</span><span class="sxs-lookup"><span data-stu-id="a545b-1194">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="a545b-1195">此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。</span><span class="sxs-lookup"><span data-stu-id="a545b-1195">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="a545b-1196">這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。</span><span class="sxs-lookup"><span data-stu-id="a545b-1196">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="a545b-1197">透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。</span><span class="sxs-lookup"><span data-stu-id="a545b-1197">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="a545b-1198">`Certificate` 區段是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="a545b-1198">The `Certificate` section is optional.</span></span> <span data-ttu-id="a545b-1199">如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。</span><span class="sxs-lookup"><span data-stu-id="a545b-1199">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="a545b-1200">如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。</span><span class="sxs-lookup"><span data-stu-id="a545b-1200">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="a545b-1201">`Certificate`一節同時支援**路徑** &ndash; **密碼**和**主體** &ndash; **存放區**憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1201">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="a545b-1202">可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。</span><span class="sxs-lookup"><span data-stu-id="a545b-1202">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="a545b-1203">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, listenOptions => { })` 方法，此方法可用來補充已設定的端點設定：</span><span class="sxs-lookup"><span data-stu-id="a545b-1203">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="a545b-1204">`KestrelServerOptions.ConfigurationLoader`可以直接存取，以繼續逐一查看現有的載入器，例如所提供的載入器 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 。</span><span class="sxs-lookup"><span data-stu-id="a545b-1204">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="a545b-1205">您可以在方法的選項中取得每個端點的設定區段， `Endpoint` 以便讀取自訂設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-1205">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="a545b-1206">可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 而載入多個組態。</span><span class="sxs-lookup"><span data-stu-id="a545b-1206">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="a545b-1207">只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1207">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="a545b-1208">中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。</span><span class="sxs-lookup"><span data-stu-id="a545b-1208">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="a545b-1209">`KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。</span><span class="sxs-lookup"><span data-stu-id="a545b-1209">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="a545b-1210">這些多載不使用名稱，並且只使用來自組態的預設組態。</span><span class="sxs-lookup"><span data-stu-id="a545b-1210">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="a545b-1211">*變更程式碼中的預設值*</span><span class="sxs-lookup"><span data-stu-id="a545b-1211">*Change the defaults in code*</span></span>

<span data-ttu-id="a545b-1212">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1212">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="a545b-1213">`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="a545b-1213">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="a545b-1214">*SNI 的 Kestrel 支援*</span><span class="sxs-lookup"><span data-stu-id="a545b-1214">*Kestrel support for SNI*</span></span>

<span data-ttu-id="a545b-1215">[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。</span><span class="sxs-lookup"><span data-stu-id="a545b-1215">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="a545b-1216">SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1216">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="a545b-1217">用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。</span><span class="sxs-lookup"><span data-stu-id="a545b-1217">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="a545b-1218">Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。</span><span class="sxs-lookup"><span data-stu-id="a545b-1218">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="a545b-1219">回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1219">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="a545b-1220">SNI 支援需要：</span><span class="sxs-lookup"><span data-stu-id="a545b-1220">SNI support requires:</span></span>

* <span data-ttu-id="a545b-1221">在目標 framework `netcoreapp2.1` 或更新版本上執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-1221">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="a545b-1222">在 `net461` 或更新版本上，會叫用回呼，但 `name` 一律為 `null` 。</span><span class="sxs-lookup"><span data-stu-id="a545b-1222">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="a545b-1223">如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1223">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="a545b-1224">所有網站都在相同的 Kestrel 執行個體上執行。</span><span class="sxs-lookup"><span data-stu-id="a545b-1224">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="a545b-1225">在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-1225">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="a545b-1226">連接記錄</span><span class="sxs-lookup"><span data-stu-id="a545b-1226">Connection logging</span></span>

<span data-ttu-id="a545b-1227">呼叫 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> ，以針對連接上的位元組層級通訊發出調試層級記錄。</span><span class="sxs-lookup"><span data-stu-id="a545b-1227">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="a545b-1228">連線記錄有助於疑難排解低層級通訊中的問題，例如在 TLS 加密期間和 proxy 後方。</span><span class="sxs-lookup"><span data-stu-id="a545b-1228">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="a545b-1229">如果 `UseConnectionLogging` 放在之前 `UseHttps` ，則會記錄加密的流量。</span><span class="sxs-lookup"><span data-stu-id="a545b-1229">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="a545b-1230">如果 `UseConnectionLogging` 放在之後 `UseHttps` ，則會記錄解密的流量。</span><span class="sxs-lookup"><span data-stu-id="a545b-1230">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="a545b-1231">繫結至 TCP 通訊端</span><span class="sxs-lookup"><span data-stu-id="a545b-1231">Bind to a TCP socket</span></span>

<span data-ttu-id="a545b-1232"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 方法會繫結至 TCP 通訊端，而選項 Lambda 則會允許 X.509 憑證設定：</span><span class="sxs-lookup"><span data-stu-id="a545b-1232">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="a545b-1233">此範例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>來為端點設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-1233">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="a545b-1234">若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。</span><span class="sxs-lookup"><span data-stu-id="a545b-1234">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="a545b-1235">繫結至 Unix 通訊端</span><span class="sxs-lookup"><span data-stu-id="a545b-1235">Bind to a Unix socket</span></span>

<span data-ttu-id="a545b-1236">請使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> 在 Unix 通訊端上進行接聽以改善 Nginx 的效能，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="a545b-1236">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

* <span data-ttu-id="a545b-1237">在 Nginx confiuguration 檔中，將 `server`  >  `location`  >  `proxy_pass` 專案設定為 `http://unix:/tmp/{KESTREL SOCKET}:/;` 。</span><span class="sxs-lookup"><span data-stu-id="a545b-1237">In the Nginx confiuguration file, set the `server` > `location` > `proxy_pass` entry to `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span></span> <span data-ttu-id="a545b-1238">`{KESTREL SOCKET}`這是提供給的通訊端名稱 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> （例如， `kestrel-test.sock` 在上述範例中為）。</span><span class="sxs-lookup"><span data-stu-id="a545b-1238">`{KESTREL SOCKET}` is the name of the socket provided to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> (for example, `kestrel-test.sock` in the preceding example).</span></span>
* <span data-ttu-id="a545b-1239">請確定通訊端可由 Nginx 寫入（例如， `chmod go+w /tmp/kestrel-test.sock` ）。</span><span class="sxs-lookup"><span data-stu-id="a545b-1239">Ensure that the socket is writeable by Nginx (for example, `chmod go+w /tmp/kestrel-test.sock`).</span></span> 

### <a name="port-0"></a><span data-ttu-id="a545b-1240">連接埠 0</span><span class="sxs-lookup"><span data-stu-id="a545b-1240">Port 0</span></span>

<span data-ttu-id="a545b-1241">指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="a545b-1241">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="a545b-1242">下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：</span><span class="sxs-lookup"><span data-stu-id="a545b-1242">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="a545b-1243">當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：</span><span class="sxs-lookup"><span data-stu-id="a545b-1243">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="a545b-1244">限制</span><span class="sxs-lookup"><span data-stu-id="a545b-1244">Limitations</span></span>

<span data-ttu-id="a545b-1245">使用下列方法來設定端點：</span><span class="sxs-lookup"><span data-stu-id="a545b-1245">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="a545b-1246">`--urls` 命令列引數</span><span class="sxs-lookup"><span data-stu-id="a545b-1246">`--urls` command-line argument</span></span>
* <span data-ttu-id="a545b-1247">`urls` 主機組態索引鍵</span><span class="sxs-lookup"><span data-stu-id="a545b-1247">`urls` host configuration key</span></span>
* <span data-ttu-id="a545b-1248">`ASPNETCORE_URLS` 環境變數</span><span class="sxs-lookup"><span data-stu-id="a545b-1248">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="a545b-1249">要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="a545b-1249">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="a545b-1250">不過，請注意下列限制：</span><span class="sxs-lookup"><span data-stu-id="a545b-1250">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="a545b-1251">HTTPS 無法與這些方法搭配使用，除非在 HTTPS 端點設定中提供預設憑證 (例如，使用 `KestrelServerOptions` 設定或設定檔，如本主題稍早所示)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1251">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="a545b-1252">當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。</span><span class="sxs-lookup"><span data-stu-id="a545b-1252">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="a545b-1253">IIS 端點設定</span><span class="sxs-lookup"><span data-stu-id="a545b-1253">IIS endpoint configuration</span></span>

<span data-ttu-id="a545b-1254">使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-1254">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="a545b-1255">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。</span><span class="sxs-lookup"><span data-stu-id="a545b-1255">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="a545b-1256">傳輸組態</span><span class="sxs-lookup"><span data-stu-id="a545b-1256">Transport configuration</span></span>

<span data-ttu-id="a545b-1257">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="a545b-1257">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="a545b-1258">對於升級到 2.1 且會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> 並相依於下列任一套件的 ASP.NET Core 2.0 應用程式來說，這是一項中斷性變更：</span><span class="sxs-lookup"><span data-stu-id="a545b-1258">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="a545b-1259">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)</span><span class="sxs-lookup"><span data-stu-id="a545b-1259">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="a545b-1260">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="a545b-1260">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="a545b-1261">針對需要使用 Libuv 的專案：</span><span class="sxs-lookup"><span data-stu-id="a545b-1261">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="a545b-1262">將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：</span><span class="sxs-lookup"><span data-stu-id="a545b-1262">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="a545b-1263">呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>：</span><span class="sxs-lookup"><span data-stu-id="a545b-1263">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="a545b-1264">URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="a545b-1264">URL prefixes</span></span>

<span data-ttu-id="a545b-1265">使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。</span><span class="sxs-lookup"><span data-stu-id="a545b-1265">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="a545b-1266">只有 HTTP URL 前置詞有效。</span><span class="sxs-lookup"><span data-stu-id="a545b-1266">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="a545b-1267">使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a545b-1267">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="a545b-1268">IPv4 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-1268">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="a545b-1269">`0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。</span><span class="sxs-lookup"><span data-stu-id="a545b-1269">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="a545b-1270">IPv6 位址與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-1270">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="a545b-1271">`[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。</span><span class="sxs-lookup"><span data-stu-id="a545b-1271">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="a545b-1272">主機名稱與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-1272">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="a545b-1273">主機名稱 `*` 和 `+` 並不特殊。</span><span class="sxs-lookup"><span data-stu-id="a545b-1273">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="a545b-1274">無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="a545b-1274">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="a545b-1275">若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1275">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="a545b-1276">裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1276">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="a545b-1277">主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼</span><span class="sxs-lookup"><span data-stu-id="a545b-1277">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="a545b-1278">如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。</span><span class="sxs-lookup"><span data-stu-id="a545b-1278">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="a545b-1279">如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="a545b-1279">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="a545b-1280">如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。</span><span class="sxs-lookup"><span data-stu-id="a545b-1280">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="a545b-1281">主機篩選</span><span class="sxs-lookup"><span data-stu-id="a545b-1281">Host filtering</span></span>

<span data-ttu-id="a545b-1282">雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。</span><span class="sxs-lookup"><span data-stu-id="a545b-1282">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="a545b-1283">主機 `localhost` 是特殊情況，用來繫結到回送位址。</span><span class="sxs-lookup"><span data-stu-id="a545b-1283">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="a545b-1284">任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="a545b-1284">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="a545b-1285">`Host` 標頭未驗證。</span><span class="sxs-lookup"><span data-stu-id="a545b-1285">`Host` headers aren't validated.</span></span>

<span data-ttu-id="a545b-1286">因應措施是使用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a545b-1286">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="a545b-1287">主機篩選中介軟體是由[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)（ASP.NET Core 2.1 或2.2）所包含的[HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering)套件所提供。</span><span class="sxs-lookup"><span data-stu-id="a545b-1287">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="a545b-1288">中介軟體是由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 所新增，它會呼叫 <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>：</span><span class="sxs-lookup"><span data-stu-id="a545b-1288">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="a545b-1289">預設停用主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a545b-1289">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="a545b-1290">若要啟用中介軟體，請 `AllowedHosts` 在*appsettings*中定義金鑰 / *appsettings。 \<EnvironmentName>json*。</span><span class="sxs-lookup"><span data-stu-id="a545b-1290">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="a545b-1291">此值是以分號分隔的主機名稱清單，不含連接埠號碼：</span><span class="sxs-lookup"><span data-stu-id="a545b-1291">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="a545b-1292">*appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="a545b-1292">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="a545b-1293">[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> 選項。</span><span class="sxs-lookup"><span data-stu-id="a545b-1293">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="a545b-1294">在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="a545b-1294">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="a545b-1295">當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1295">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="a545b-1296">當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。</span><span class="sxs-lookup"><span data-stu-id="a545b-1296">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="a545b-1297">如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="a545b-1297">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="http11-request-draining"></a><span data-ttu-id="a545b-1298">HTTP/1.1 要求清空</span><span class="sxs-lookup"><span data-stu-id="a545b-1298">HTTP/1.1 request draining</span></span>

<span data-ttu-id="a545b-1299">開啟 HTTP 連接非常耗時。</span><span class="sxs-lookup"><span data-stu-id="a545b-1299">Opening HTTP connections is time consuming.</span></span> <span data-ttu-id="a545b-1300">針對 HTTPS，這也是耗用大量資源。</span><span class="sxs-lookup"><span data-stu-id="a545b-1300">For HTTPS, it's also resource intensive.</span></span> <span data-ttu-id="a545b-1301">因此，Kestrel 會嘗試每個 HTTP/1.1 通訊協定重複使用連接。</span><span class="sxs-lookup"><span data-stu-id="a545b-1301">Therefore, Kestrel tries to reuse connections per the HTTP/1.1 protocol.</span></span> <span data-ttu-id="a545b-1302">要求主體必須完全取用，才能重複使用連接。</span><span class="sxs-lookup"><span data-stu-id="a545b-1302">A request body must be fully consumed to allow the connection to be reused.</span></span> <span data-ttu-id="a545b-1303">應用程式不一定會使用要求主體，例如伺服器傳回重新 `POST` 導向或404回應的要求。</span><span class="sxs-lookup"><span data-stu-id="a545b-1303">The app doesn't always consume the request body, such as a `POST` requests where the server returns a redirect or 404 response.</span></span> <span data-ttu-id="a545b-1304">在 `POST` -重新導向案例中：</span><span class="sxs-lookup"><span data-stu-id="a545b-1304">In the `POST`-redirect case:</span></span>

* <span data-ttu-id="a545b-1305">用戶端可能已經傳送部分 `POST` 資料。</span><span class="sxs-lookup"><span data-stu-id="a545b-1305">The client may already have sent part of the `POST` data.</span></span>
* <span data-ttu-id="a545b-1306">伺服器會寫入301回應。</span><span class="sxs-lookup"><span data-stu-id="a545b-1306">The server writes the 301 response.</span></span>
* <span data-ttu-id="a545b-1307">連接無法用於新的要求，直到 `POST` 先前要求主體的資料完全讀取為止。</span><span class="sxs-lookup"><span data-stu-id="a545b-1307">The connection can't be used for a new request until the `POST` data from the previous request body has been fully read.</span></span>
* <span data-ttu-id="a545b-1308">Kestrel 嘗試清空要求主體。</span><span class="sxs-lookup"><span data-stu-id="a545b-1308">Kestrel tries to drain the request body.</span></span> <span data-ttu-id="a545b-1309">清空要求本文表示讀取和捨棄資料，而不進行處理。</span><span class="sxs-lookup"><span data-stu-id="a545b-1309">Draining the request body means reading and discarding the data without processing it.</span></span>

<span data-ttu-id="a545b-1310">清空程式可讓您在允許重複使用連線時，以及釋放任何剩餘資料所需的時間之間進行 tradoff：</span><span class="sxs-lookup"><span data-stu-id="a545b-1310">The draining process makes a tradoff between allowing the connection to be reused and the time it takes to drain any remaining data:</span></span>

* <span data-ttu-id="a545b-1311">清空的超時時間為5秒，無法設定。</span><span class="sxs-lookup"><span data-stu-id="a545b-1311">Draining has a timeout of five seconds, which isn't configurable.</span></span>
* <span data-ttu-id="a545b-1312">如果或標頭所指定的所有資料都 `Content-Length` `Transfer-Encoding` 未在超時之前讀取，連接就會關閉。</span><span class="sxs-lookup"><span data-stu-id="a545b-1312">If all of the data specified by the `Content-Length` or `Transfer-Encoding` header hasn't been read before the timeout, the connection is closed.</span></span>

<span data-ttu-id="a545b-1313">有時候，您可能會想要在寫入回應之前或之後立即終止要求。</span><span class="sxs-lookup"><span data-stu-id="a545b-1313">Sometimes you may want to terminate the request immediately, before or after writing the response.</span></span> <span data-ttu-id="a545b-1314">例如，用戶端可能會有限制的資料上限，因此限制上傳的資料可能是優先順序。</span><span class="sxs-lookup"><span data-stu-id="a545b-1314">For example, clients may have restrictive data caps, so limiting uploaded data might be a priority.</span></span> <span data-ttu-id="a545b-1315">在這種情況下，若要終止要求，請從控制器、頁面或中介軟體中呼叫[HttpCoNtext. Abort。](xref:Microsoft.AspNetCore.Http.HttpContext.Abort%2A) Razor</span><span class="sxs-lookup"><span data-stu-id="a545b-1315">In such cases to terminate a request, call [HttpContext.Abort](xref:Microsoft.AspNetCore.Http.HttpContext.Abort%2A) from a controller, Razor Page, or middleware.</span></span>

<span data-ttu-id="a545b-1316">呼叫有幾點注意事項 `Abort` ：</span><span class="sxs-lookup"><span data-stu-id="a545b-1316">There are caveats to calling `Abort`:</span></span>

* <span data-ttu-id="a545b-1317">建立新的連接可能會變慢且昂貴。</span><span class="sxs-lookup"><span data-stu-id="a545b-1317">Creating new connections can be slow and expensive.</span></span>
* <span data-ttu-id="a545b-1318">不保證用戶端在連接關閉之前已讀取回應。</span><span class="sxs-lookup"><span data-stu-id="a545b-1318">There's no guarantee that the client has read the response before the connection closes.</span></span>
* <span data-ttu-id="a545b-1319">呼叫 `Abort` 應該很罕見，並保留給嚴重的錯誤案例，而不是常見的錯誤。</span><span class="sxs-lookup"><span data-stu-id="a545b-1319">Calling `Abort` should be rare and reserved for severe error cases, not common errors.</span></span>
  * <span data-ttu-id="a545b-1320">只有 `Abort` 在需要解決特定問題時，才呼叫。</span><span class="sxs-lookup"><span data-stu-id="a545b-1320">Only call `Abort` when a specific problem needs to be solved.</span></span> <span data-ttu-id="a545b-1321">例如， `Abort` 如果惡意用戶端正在嘗試資料， `POST` 或當用戶端程式代碼中發生錯誤而導致大型或多個要求時，請呼叫。</span><span class="sxs-lookup"><span data-stu-id="a545b-1321">For example, call `Abort` if malicious clients are trying to `POST` data or when there's a bug in client code that causes large or numerous requests.</span></span>
  * <span data-ttu-id="a545b-1322">請勿呼叫 `Abort` 常見的錯誤狀況，例如 HTTP 404 （找不到）。</span><span class="sxs-lookup"><span data-stu-id="a545b-1322">Don't call `Abort` for common error situations, such as HTTP 404 (Not Found).</span></span>

<span data-ttu-id="a545b-1323">呼叫[HttpResponse](xref:Microsoft.AspNetCore.Http.HttpResponse.CompleteAsync%2A)之前，請先呼叫 CompleteAsync `Abort` ，以確保伺服器已完成寫入回應。</span><span class="sxs-lookup"><span data-stu-id="a545b-1323">Calling [HttpResponse.CompleteAsync](xref:Microsoft.AspNetCore.Http.HttpResponse.CompleteAsync%2A) before calling `Abort` ensures that the server has completed writing the response.</span></span> <span data-ttu-id="a545b-1324">不過，用戶端行為無法預測，而且在中斷連線之前，它們可能不會讀取回應。</span><span class="sxs-lookup"><span data-stu-id="a545b-1324">However, client behavior isn't predictable and they may not read the response before the connection is aborted.</span></span>

<span data-ttu-id="a545b-1325">此程式與 HTTP/2 不同，因為通訊協定支援在不關閉連線的情況下中止個別要求資料流程。</span><span class="sxs-lookup"><span data-stu-id="a545b-1325">This process is different for HTTP/2 because the protocol supports aborting individual request streams without closing the connection.</span></span> <span data-ttu-id="a545b-1326">五秒的清空超時不適用。</span><span class="sxs-lookup"><span data-stu-id="a545b-1326">The five second drain timeout doesn't apply.</span></span> <span data-ttu-id="a545b-1327">如果在完成回應之後有任何未讀取的要求本文資料，伺服器就會傳送 HTTP/2 RST 框架。</span><span class="sxs-lookup"><span data-stu-id="a545b-1327">If there's any unread request body data after completing a response, then the server sends an HTTP/2 RST frame.</span></span> <span data-ttu-id="a545b-1328">系統會忽略其他要求主體資料框架。</span><span class="sxs-lookup"><span data-stu-id="a545b-1328">Additional request body data frames are ignored.</span></span>

<span data-ttu-id="a545b-1329">可能的話，用戶端最好先利用[預期的： 100-continue](https://developer.mozilla.org/docs/Web/HTTP/Status/100)要求標頭，等待伺服器回應，然後再開始傳送要求本文。</span><span class="sxs-lookup"><span data-stu-id="a545b-1329">If possible, it's better for clients to utilize the [Expect: 100-continue](https://developer.mozilla.org/docs/Web/HTTP/Status/100) request header and wait for the server to respond before starting to send the request body.</span></span> <span data-ttu-id="a545b-1330">這讓用戶端有機會在傳送不需要的資料之前，先檢查回應並中止。</span><span class="sxs-lookup"><span data-stu-id="a545b-1330">That gives the client an opportunity to examine the response and abort before sending unneeded data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a545b-1331">其他資源</span><span class="sxs-lookup"><span data-stu-id="a545b-1331">Additional resources</span></span>

* <span data-ttu-id="a545b-1332">在 Linux 上使用 UNIX 通訊端時，通訊端不會在應用程式關閉時自動刪除。</span><span class="sxs-lookup"><span data-stu-id="a545b-1332">When using UNIX sockets on Linux, the socket is not automatically deleted on app shut down.</span></span> <span data-ttu-id="a545b-1333">如需詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/aspnetcore/issues/14134)。</span><span class="sxs-lookup"><span data-stu-id="a545b-1333">For more information, see [this GitHub issue](https://github.com/dotnet/aspnetcore/issues/14134).</span></span>
* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="a545b-1334">RFC 7230：訊息語法和路由 (第 5.4 節：主機)</span><span class="sxs-lookup"><span data-stu-id="a545b-1334">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
