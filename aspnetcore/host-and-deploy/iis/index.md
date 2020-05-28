---
<span data-ttu-id="d049d-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-102">'Blazor'</span></span>
- <span data-ttu-id="d049d-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-103">'Identity'</span></span>
- <span data-ttu-id="d049d-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-105">'Razor'</span></span>
- <span data-ttu-id="d049d-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-106">'SignalR' uid:</span></span> 

---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="d049d-107">在使用 IIS 的 Windows 上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d049d-107">Host ASP.NET Core on Windows with IIS</span></span>

<!-- 

    NOTE FOR 5.0
    
    When making the 5.0 version of this topic, remove the Hosting Bundle
    direct download section from the (new) <5.0 & >2.2 version and modify 
    the text and heading for the *Earlier versions of the installer* 
    section. See the 2.2 version for an example.
    
-->

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d049d-108">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="d049d-108">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="d049d-109">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="d049d-109">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="d049d-110">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="d049d-110">Supported operating systems</span></span>

<span data-ttu-id="d049d-111">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="d049d-111">The following operating systems are supported:</span></span>

* <span data-ttu-id="d049d-112">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-112">Windows 7 or later</span></span>
* <span data-ttu-id="d049d-113">Windows Server 2012 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-113">Windows Server 2012 R2 or later</span></span>

<span data-ttu-id="d049d-114">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-114">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="d049d-115">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="d049d-115">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="d049d-116">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="d049d-116">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="d049d-117">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="d049d-117">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="d049d-118">支援的平台</span><span class="sxs-lookup"><span data-stu-id="d049d-118">Supported platforms</span></span>

<span data-ttu-id="d049d-119">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-119">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="d049d-120">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="d049d-120">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="d049d-121">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="d049d-121">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="d049d-122">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="d049d-122">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="d049d-123">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="d049d-123">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="d049d-124">針對32位（x86）發行的應用程式必須為其 IIS 應用程式集區啟用32位。</span><span class="sxs-lookup"><span data-stu-id="d049d-124">Apps published for 32-bit (x86) must have 32-bit enabled for their IIS Application Pools.</span></span> <span data-ttu-id="d049d-125">如需詳細資訊，請參閱[建立 IIS 網站](#create-the-iis-site)一節。</span><span class="sxs-lookup"><span data-stu-id="d049d-125">For more information, see the [Create the IIS site](#create-the-iis-site) section.</span></span>

<span data-ttu-id="d049d-126">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-126">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="d049d-127">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="d049d-127">A 64-bit runtime must be present on the host system.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="d049d-128">裝載模型</span><span class="sxs-lookup"><span data-stu-id="d049d-128">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="d049d-129">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="d049d-129">In-process hosting model</span></span>

<span data-ttu-id="d049d-130">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="d049d-130">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="d049d-131">因為要求未透過回送介面卡 (將連出網路流量傳回同一部電腦的網路介面) 進行 proxy 處理，所以同處理序裝載會提供優於跨處理序裝載的效能。</span><span class="sxs-lookup"><span data-stu-id="d049d-131">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="d049d-132">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="d049d-132">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="d049d-133">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)：</span><span class="sxs-lookup"><span data-stu-id="d049d-133">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="d049d-134">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="d049d-134">Performs app initialization.</span></span>
  * <span data-ttu-id="d049d-135">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="d049d-135">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="d049d-136">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="d049d-136">Calls `Program.Main`.</span></span>
* <span data-ttu-id="d049d-137">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="d049d-137">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="d049d-138">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="d049d-138">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-inprocess.png)

1. <span data-ttu-id="d049d-140">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-140">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span>
1. <span data-ttu-id="d049d-141">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="d049d-141">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span>
1. <span data-ttu-id="d049d-142">ASP.NET Core 模組會接收原生要求，並將它傳遞至 IIS HTTP 伺服器（ `IISHttpServer` ）。</span><span class="sxs-lookup"><span data-stu-id="d049d-142">The ASP.NET Core Module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="d049d-143">IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。</span><span class="sxs-lookup"><span data-stu-id="d049d-143">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="d049d-144">在 IIS HTTP 伺服器處理要求之後：</span><span class="sxs-lookup"><span data-stu-id="d049d-144">After the IIS HTTP Server processes the request:</span></span>

1. <span data-ttu-id="d049d-145">要求會傳送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="d049d-145">The request is sent to the ASP.NET Core middleware pipeline.</span></span>
1. <span data-ttu-id="d049d-146">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d049d-146">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span>
1. <span data-ttu-id="d049d-147">應用程式的回應會透過 IIS HTTP 伺服器傳回 IIS。</span><span class="sxs-lookup"><span data-stu-id="d049d-147">The app's response is passed back to IIS through IIS HTTP Server.</span></span>
1. <span data-ttu-id="d049d-148">IIS 會將回應傳送到起始該要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="d049d-148">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="d049d-149">同進程裝載是加入宣告現有的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-149">In-process hosting is opt-in for existing apps.</span></span> <span data-ttu-id="d049d-150">ASP.NET Core 的 web 範本會使用同進程裝載模型。</span><span class="sxs-lookup"><span data-stu-id="d049d-150">The ASP.NET Core web templates use the in-process hosting model.</span></span>

<span data-ttu-id="d049d-151">`CreateDefaultBuilder` 會透過呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 方法來啟動 [CoreCLR](/dotnet/standard/glossary#coreclr) 以新增 <xref:Microsoft.AspNetCore.Hosting.Server.IServer>，並在 IIS 工作者處理序 (*w3wp.exe* 或 *iisexpress.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-151">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="d049d-152">效能測試指出，相較於將 .NET Core 應用程式裝載於處理序外，並將要求 Proxy 處理至 [Kestrel](xref:fundamentals/servers/kestrel)，裝載於處理序內可提供明顯更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="d049d-152">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="d049d-153">發佈為單一檔案可執行檔的應用程式無法由同處理序裝載模型載入。</span><span class="sxs-lookup"><span data-stu-id="d049d-153">Apps published as a single file executable can't be loaded by the in-process hosting model.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="d049d-154">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="d049d-154">Out-of-process hosting model</span></span>

<span data-ttu-id="d049d-155">因為 ASP.NET Core 應用程式會在與 IIS 背景工作進程不同的進程中執行，所以 ASP.NET Core 模組會處理進程管理。</span><span class="sxs-lookup"><span data-stu-id="d049d-155">Because ASP.NET Core apps run in a process separate from the IIS worker process, the ASP.NET Core Module handles process management.</span></span> <span data-ttu-id="d049d-156">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="d049d-156">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="d049d-157">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="d049d-157">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="d049d-158">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="d049d-158">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![非同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-outofprocess.png)

1. <span data-ttu-id="d049d-160">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-160">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span>
1. <span data-ttu-id="d049d-161">驅動程式會將要求路由至網站設定之埠上的 IIS。</span><span class="sxs-lookup"><span data-stu-id="d049d-161">The driver routes the requests to IIS on the website's configured port.</span></span> <span data-ttu-id="d049d-162">設定的埠通常是80（HTTP）或443（HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="d049d-162">The configured port is usually 80 (HTTP) or 443 (HTTPS).</span></span>
1. <span data-ttu-id="d049d-163">模組會在應用程式的隨機埠上將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d049d-163">The module forwards the requests to Kestrel on a random port for the app.</span></span> <span data-ttu-id="d049d-164">隨機埠不是80或443。</span><span class="sxs-lookup"><span data-stu-id="d049d-164">The random port isn't 80 or 443.</span></span>

<!-- make this a bullet list -->
<span data-ttu-id="d049d-165">ASP.NET Core 模組會在啟動時透過環境變數指定埠。</span><span class="sxs-lookup"><span data-stu-id="d049d-165">The ASP.NET Core Module specifies the port via an environment variable at startup.</span></span> <span data-ttu-id="d049d-166">延伸模組會設定 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 伺服器來接聽 `http://localhost:{PORT}` 。</span><span class="sxs-lookup"><span data-stu-id="d049d-166">The <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="d049d-167">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="d049d-167">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="d049d-168">模組不支援 HTTPS 轉送。</span><span class="sxs-lookup"><span data-stu-id="d049d-168">The module doesn't support HTTPS forwarding.</span></span> <span data-ttu-id="d049d-169">即使 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="d049d-169">Requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="d049d-170">在 Kestrel 拾取來自模組的要求之後，會將要求轉送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="d049d-170">After Kestrel picks up the request from the module, the request is forwarded into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="d049d-171">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d049d-171">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="d049d-172">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d049d-172">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="d049d-173">應用程式的回應會傳回 IIS，將它轉送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="d049d-173">The app's response is passed back to IIS, which forwards it back to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="d049d-174">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="d049d-174">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="d049d-175">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="d049d-175">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="d049d-176">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="d049d-176">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="d049d-177">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="d049d-177">Enable the IISIntegration components</span></span>

<span data-ttu-id="d049d-178">在 `CreateHostBuilder` （*Program.cs*）中建立主機時，請呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="d049d-178">When building a host in `CreateHostBuilder` (*Program.cs*), call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="d049d-179">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/generic-host#default-builder-settings>。</span><span class="sxs-lookup"><span data-stu-id="d049d-179">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/generic-host#default-builder-settings>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="d049d-180">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="d049d-180">IIS options</span></span>

<span data-ttu-id="d049d-181">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="d049d-181">**In-process hosting model**</span></span>

<span data-ttu-id="d049d-182">若要設定 IIS 伺服器選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISServerOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-182">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="d049d-183">下列範例會停用 AutomaticAuthentication：</span><span class="sxs-lookup"><span data-stu-id="d049d-183">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="d049d-184">選項</span><span class="sxs-lookup"><span data-stu-id="d049d-184">Option</span></span>                         | <span data-ttu-id="d049d-185">預設</span><span class="sxs-lookup"><span data-stu-id="d049d-185">Default</span></span> | <span data-ttu-id="d049d-186">設定</span><span class="sxs-lookup"><span data-stu-id="d049d-186">Setting</span></span> |
| ---
<span data-ttu-id="d049d-187">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-187">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-188">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-188">'Blazor'</span></span>
- <span data-ttu-id="d049d-189">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-189">'Identity'</span></span>
- <span data-ttu-id="d049d-190">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-190">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-191">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-191">'Razor'</span></span>
- <span data-ttu-id="d049d-192">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-192">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-193">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-193">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-194">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-194">'Blazor'</span></span>
- <span data-ttu-id="d049d-195">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-195">'Identity'</span></span>
- <span data-ttu-id="d049d-196">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-196">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-197">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-197">'Razor'</span></span>
- <span data-ttu-id="d049d-198">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-198">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-199">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-199">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-200">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-200">'Blazor'</span></span>
- <span data-ttu-id="d049d-201">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-201">'Identity'</span></span>
- <span data-ttu-id="d049d-202">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-202">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-203">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-203">'Razor'</span></span>
- <span data-ttu-id="d049d-204">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-204">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-205">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-205">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-206">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-206">'Blazor'</span></span>
- <span data-ttu-id="d049d-207">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-207">'Identity'</span></span>
- <span data-ttu-id="d049d-208">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-208">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-209">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-209">'Razor'</span></span>
- <span data-ttu-id="d049d-210">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-210">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-211">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-211">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-212">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-212">'Blazor'</span></span>
- <span data-ttu-id="d049d-213">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-213">'Identity'</span></span>
- <span data-ttu-id="d049d-214">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-214">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-215">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-215">'Razor'</span></span>
- <span data-ttu-id="d049d-216">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-216">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-217">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-217">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-218">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-218">'Blazor'</span></span>
- <span data-ttu-id="d049d-219">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-219">'Identity'</span></span>
- <span data-ttu-id="d049d-220">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-220">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-221">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-221">'Razor'</span></span>
- <span data-ttu-id="d049d-222">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-222">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-223">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-223">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-224">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-224">'Blazor'</span></span>
- <span data-ttu-id="d049d-225">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-225">'Identity'</span></span>
- <span data-ttu-id="d049d-226">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-226">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-227">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-227">'Razor'</span></span>
- <span data-ttu-id="d049d-228">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-228">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-229">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-229">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-230">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-230">'Blazor'</span></span>
- <span data-ttu-id="d049d-231">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-231">'Identity'</span></span>
- <span data-ttu-id="d049d-232">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-232">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-233">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-233">'Razor'</span></span>
- <span data-ttu-id="d049d-234">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-234">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-235">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-235">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-236">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-236">'Blazor'</span></span>
- <span data-ttu-id="d049d-237">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-237">'Identity'</span></span>
- <span data-ttu-id="d049d-238">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-238">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-239">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-239">'Razor'</span></span>
- <span data-ttu-id="d049d-240">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-240">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-241">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-241">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-242">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-242">'Blazor'</span></span>
- <span data-ttu-id="d049d-243">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-243">'Identity'</span></span>
- <span data-ttu-id="d049d-244">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-244">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-245">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-245">'Razor'</span></span>
- <span data-ttu-id="d049d-246">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-246">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-247">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-247">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-248">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-248">'Blazor'</span></span>
- <span data-ttu-id="d049d-249">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-249">'Identity'</span></span>
- <span data-ttu-id="d049d-250">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-250">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-251">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-251">'Razor'</span></span>
- <span data-ttu-id="d049d-252">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-252">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-253">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-253">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-254">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-254">'Blazor'</span></span>
- <span data-ttu-id="d049d-255">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-255">'Identity'</span></span>
- <span data-ttu-id="d049d-256">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-256">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-257">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-257">'Razor'</span></span>
- <span data-ttu-id="d049d-258">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-258">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-259">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-259">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-260">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-260">'Blazor'</span></span>
- <span data-ttu-id="d049d-261">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-261">'Identity'</span></span>
- <span data-ttu-id="d049d-262">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-262">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-263">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-263">'Razor'</span></span>
- <span data-ttu-id="d049d-264">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-264">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-265">--------------- |:-----: |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-265">--------------- | :-----: | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-266">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-266">'Blazor'</span></span>
- <span data-ttu-id="d049d-267">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-267">'Identity'</span></span>
- <span data-ttu-id="d049d-268">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-268">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-269">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-269">'Razor'</span></span>
- <span data-ttu-id="d049d-270">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-270">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-271">---- | |`AutomaticAuthentication`      | `true` |若 `true` 為，IIS 伺服器會設定 `HttpContext.User` 由[Windows 驗證](xref:security/authentication/windowsauth)所驗證的。</span><span class="sxs-lookup"><span data-stu-id="d049d-271">---- | | `AutomaticAuthentication`      | `true`  | If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="d049d-272">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="d049d-272">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="d049d-273">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="d049d-273">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="d049d-274">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="d049d-274">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="d049d-275">| |`AuthenticationDisplayName`    | `null` |設定使用者在登入頁面上顯示的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="d049d-275">| | `AuthenticationDisplayName`    | `null`  | Sets the display name shown to users on login pages.</span></span> <span data-ttu-id="d049d-276">| |`AllowSynchronousIO`           | `false`|是否允許和的同步 i/o `HttpContext.Request` `HttpContext.Response` 。</span><span class="sxs-lookup"><span data-stu-id="d049d-276">| | `AllowSynchronousIO`           | `false` | Whether synchronous I/O is allowed for the `HttpContext.Request` and the `HttpContext.Response`.</span></span> <span data-ttu-id="d049d-277">| |`MaxRequestBodySize`           | `30000000` |取得或設定的最大要求主體大小 `HttpRequest` 。</span><span class="sxs-lookup"><span data-stu-id="d049d-277">| | `MaxRequestBodySize`           | `30000000`  | Gets or sets the max request body size for the `HttpRequest`.</span></span> <span data-ttu-id="d049d-278">請注意，IIS 本身具有限制 `maxAllowedContentLength`，此限制將在 `IISServerOptions` 中設定 `MaxRequestBodySize` 時處理。</span><span class="sxs-lookup"><span data-stu-id="d049d-278">Note that IIS itself has the limit `maxAllowedContentLength` which will be processed before the `MaxRequestBodySize` set in the `IISServerOptions`.</span></span> <span data-ttu-id="d049d-279">變更 `MaxRequestBodySize` 將不會影響 `maxAllowedContentLength`。</span><span class="sxs-lookup"><span data-stu-id="d049d-279">Changing the `MaxRequestBodySize` won't affect the `maxAllowedContentLength`.</span></span> <span data-ttu-id="d049d-280">若要增加 `maxAllowedContentLength`，請在 *web.config* 中新增項目，以將 `maxAllowedContentLength` 設定為較高的值。</span><span class="sxs-lookup"><span data-stu-id="d049d-280">To increase `maxAllowedContentLength`, add an entry in the *web.config* to set `maxAllowedContentLength` to a higher value.</span></span> <span data-ttu-id="d049d-281">如需更多詳細資料，請參閱[組態](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration)。</span><span class="sxs-lookup"><span data-stu-id="d049d-281">For more details, see [Configuration](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span></span> |

<span data-ttu-id="d049d-282">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="d049d-282">**Out-of-process hosting model**</span></span>

<span data-ttu-id="d049d-283">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-283">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="d049d-284">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="d049d-284">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="d049d-285">選項</span><span class="sxs-lookup"><span data-stu-id="d049d-285">Option</span></span>                         | <span data-ttu-id="d049d-286">預設</span><span class="sxs-lookup"><span data-stu-id="d049d-286">Default</span></span> | <span data-ttu-id="d049d-287">設定</span><span class="sxs-lookup"><span data-stu-id="d049d-287">Setting</span></span> |
| ---
<span data-ttu-id="d049d-288">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-288">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-289">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-289">'Blazor'</span></span>
- <span data-ttu-id="d049d-290">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-290">'Identity'</span></span>
- <span data-ttu-id="d049d-291">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-291">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-292">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-292">'Razor'</span></span>
- <span data-ttu-id="d049d-293">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-293">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-294">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-294">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-295">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-295">'Blazor'</span></span>
- <span data-ttu-id="d049d-296">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-296">'Identity'</span></span>
- <span data-ttu-id="d049d-297">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-297">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-298">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-298">'Razor'</span></span>
- <span data-ttu-id="d049d-299">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-299">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-300">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-300">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-301">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-301">'Blazor'</span></span>
- <span data-ttu-id="d049d-302">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-302">'Identity'</span></span>
- <span data-ttu-id="d049d-303">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-303">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-304">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-304">'Razor'</span></span>
- <span data-ttu-id="d049d-305">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-305">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-306">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-306">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-307">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-307">'Blazor'</span></span>
- <span data-ttu-id="d049d-308">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-308">'Identity'</span></span>
- <span data-ttu-id="d049d-309">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-309">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-310">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-310">'Razor'</span></span>
- <span data-ttu-id="d049d-311">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-311">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-312">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-312">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-313">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-313">'Blazor'</span></span>
- <span data-ttu-id="d049d-314">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-314">'Identity'</span></span>
- <span data-ttu-id="d049d-315">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-315">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-316">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-316">'Razor'</span></span>
- <span data-ttu-id="d049d-317">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-317">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-318">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-318">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-319">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-319">'Blazor'</span></span>
- <span data-ttu-id="d049d-320">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-320">'Identity'</span></span>
- <span data-ttu-id="d049d-321">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-321">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-322">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-322">'Razor'</span></span>
- <span data-ttu-id="d049d-323">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-323">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-324">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-324">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-325">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-325">'Blazor'</span></span>
- <span data-ttu-id="d049d-326">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-326">'Identity'</span></span>
- <span data-ttu-id="d049d-327">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-327">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-328">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-328">'Razor'</span></span>
- <span data-ttu-id="d049d-329">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-329">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-330">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-330">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-331">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-331">'Blazor'</span></span>
- <span data-ttu-id="d049d-332">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-332">'Identity'</span></span>
- <span data-ttu-id="d049d-333">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-333">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-334">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-334">'Razor'</span></span>
- <span data-ttu-id="d049d-335">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-335">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-336">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-336">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-337">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-337">'Blazor'</span></span>
- <span data-ttu-id="d049d-338">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-338">'Identity'</span></span>
- <span data-ttu-id="d049d-339">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-339">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-340">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-340">'Razor'</span></span>
- <span data-ttu-id="d049d-341">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-341">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-342">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-342">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-343">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-343">'Blazor'</span></span>
- <span data-ttu-id="d049d-344">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-344">'Identity'</span></span>
- <span data-ttu-id="d049d-345">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-345">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-346">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-346">'Razor'</span></span>
- <span data-ttu-id="d049d-347">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-347">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-348">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-348">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-349">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-349">'Blazor'</span></span>
- <span data-ttu-id="d049d-350">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-350">'Identity'</span></span>
- <span data-ttu-id="d049d-351">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-351">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-352">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-352">'Razor'</span></span>
- <span data-ttu-id="d049d-353">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-353">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-354">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-354">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-355">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-355">'Blazor'</span></span>
- <span data-ttu-id="d049d-356">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-356">'Identity'</span></span>
- <span data-ttu-id="d049d-357">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-357">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-358">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-358">'Razor'</span></span>
- <span data-ttu-id="d049d-359">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-359">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-360">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-360">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-361">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-361">'Blazor'</span></span>
- <span data-ttu-id="d049d-362">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-362">'Identity'</span></span>
- <span data-ttu-id="d049d-363">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-363">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-364">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-364">'Razor'</span></span>
- <span data-ttu-id="d049d-365">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-365">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-366">--------------- |:-----: |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-366">--------------- | :-----: | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-367">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-367">'Blazor'</span></span>
- <span data-ttu-id="d049d-368">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-368">'Identity'</span></span>
- <span data-ttu-id="d049d-369">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-369">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-370">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-370">'Razor'</span></span>
- <span data-ttu-id="d049d-371">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-371">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-372">---- | |`AutomaticAuthentication`      | `true` |若 `true` 為， [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定 `HttpContext.User` 由[Windows 驗證](xref:security/authentication/windowsauth)所驗證的。</span><span class="sxs-lookup"><span data-stu-id="d049d-372">---- | | `AutomaticAuthentication`      | `true`  | If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="d049d-373">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="d049d-373">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="d049d-374">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="d049d-374">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="d049d-375">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-375">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> <span data-ttu-id="d049d-376">| |`AuthenticationDisplayName`    | `null` |設定使用者在登入頁面上顯示的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="d049d-376">| | `AuthenticationDisplayName`    | `null`  | Sets the display name shown to users on login pages.</span></span> <span data-ttu-id="d049d-377">| |`ForwardClientCertificate`     | `true` |如果 `true` 和 `MS-ASPNETCORE-CLIENTCERT` 要求標頭存在，則 `HttpContext.Connection.ClientCertificate` 會填入。</span><span class="sxs-lookup"><span data-stu-id="d049d-377">| | `ForwardClientCertificate`     | `true`  | If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d049d-378">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="d049d-378">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d049d-379">[IIS 整合中介軟體](#enable-the-iisintegration-components)和 ASP.NET Core 模組已設定為轉送：</span><span class="sxs-lookup"><span data-stu-id="d049d-379">The [IIS Integration Middleware](#enable-the-iisintegration-components) and the ASP.NET Core Module are configured to forward the:</span></span>

* <span data-ttu-id="d049d-380">配置（HTTP/HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="d049d-380">Scheme (HTTP/HTTPS).</span></span>
* <span data-ttu-id="d049d-381">發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="d049d-381">Remote IP address where the request originated.</span></span>

<span data-ttu-id="d049d-382">[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定轉送的標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d049d-382">The [IIS Integration Middleware](#enable-the-iisintegration-components) configures Forwarded Headers Middleware.</span></span>

<span data-ttu-id="d049d-383">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-383">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="d049d-384">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="d049d-384">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="d049d-385">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="d049d-385">web.config file</span></span>

<span data-ttu-id="d049d-386">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="d049d-386">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="d049d-387">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="d049d-387">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="d049d-388">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="d049d-388">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="d049d-389">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="d049d-389">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="d049d-390">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 ASP.NET Core 模組，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="d049d-390">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="d049d-391">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="d049d-391">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="d049d-392">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-392">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="d049d-393">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-393">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="d049d-394">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-394">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="d049d-395">若要防止 Web SDK*轉換 web.config 檔案*，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="d049d-395">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="d049d-396">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="d049d-396">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="d049d-397">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="d049d-397">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="d049d-398">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="d049d-398">web.config file location</span></span>

<span data-ttu-id="d049d-399">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) *，web.config 檔案*必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="d049d-399">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="d049d-400">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="d049d-400">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="d049d-401">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-401">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="d049d-402">機密檔案存在於應用程式的實體路徑，例如\* \<assembly> .runtimeconfig.json. json*、 \* \<assembly> .xml* （xml 檔批註）和\* \<assembly> .deps.json\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-402">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="d049d-403">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="d049d-403">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="d049d-404">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-404">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="d049d-405">\***Web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署*移除 web.config 檔案\*。**</span><span class="sxs-lookup"><span data-stu-id="d049d-405">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="d049d-406">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="d049d-406">Transform web.config</span></span>

<span data-ttu-id="d049d-407">如果您需要在發行時轉換*web.config* ，請參閱 <xref:host-and-deploy/iis/transform-webconfig> 。</span><span class="sxs-lookup"><span data-stu-id="d049d-407">If you need to transform *web.config* on publish, see <xref:host-and-deploy/iis/transform-webconfig>.</span></span> <span data-ttu-id="d049d-408">您可能需要在 [發行] 上轉換*web.config* ，以根據設定、設定檔或環境設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="d049d-408">You might need to transform *web.config* on publish to set environment variables based on the configuration, profile, or environment.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="d049d-409">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="d049d-409">IIS configuration</span></span>

<span data-ttu-id="d049d-410">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="d049d-410">**Windows Server operating systems**</span></span>

<span data-ttu-id="d049d-411">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="d049d-411">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="d049d-412">使用來自 [管理]\*\*\*\* 功能表的 [新增角色及功能]\*\*\*\* 精靈，或是 [伺服器管理員]\*\*\*\* 中的連結。</span><span class="sxs-lookup"><span data-stu-id="d049d-412">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="d049d-413">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-413">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="d049d-415">在 [功能]\*\*\*\* 步驟之後，[角色服務]\*\*\*\* 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="d049d-415">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="d049d-416">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="d049d-416">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="d049d-418">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-418">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="d049d-419">若要啟用 Windows 驗證，請展開下列節點： [**網頁伺服器**  >  **安全性**]。</span><span class="sxs-lookup"><span data-stu-id="d049d-419">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="d049d-420">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-420">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="d049d-421">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="d049d-421">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="d049d-422">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-422">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="d049d-423">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="d049d-423">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="d049d-424">若要啟用 websocket，請展開下列節點： [**網頁伺服器**  >  **應用程式開發**]。</span><span class="sxs-lookup"><span data-stu-id="d049d-424">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="d049d-425">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-425">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="d049d-426">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="d049d-426">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="d049d-427">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="d049d-427">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="d049d-428">安裝**網頁伺服器（iis）** 角色之後，不需要重新開機伺服器/iis。</span><span class="sxs-lookup"><span data-stu-id="d049d-428">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="d049d-429">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="d049d-429">**Windows desktop operating systems**</span></span>

<span data-ttu-id="d049d-430">啟用 [IIS 管理主控台]\*\*\*\* 和 [World Wide Web 服務]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-430">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="d049d-431">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="d049d-431">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="d049d-432">開啟 [Internet Information Services]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-432">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="d049d-433">開啟 [Web 管理工具]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-433">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="d049d-434">核取 [IIS 管理主控台]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-434">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="d049d-435">[World Wide Web Services] (全球資訊網服務)\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-435">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="d049d-436">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-436">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="d049d-437">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-437">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="d049d-438">若要啟用 Windows 驗證，請展開下列節點： **World Wide Web 服務**  >  **安全性**。</span><span class="sxs-lookup"><span data-stu-id="d049d-438">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="d049d-439">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-439">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="d049d-440">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="d049d-440">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="d049d-441">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-441">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="d049d-442">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="d049d-442">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="d049d-443">若要啟用 websocket，請展開下列節點： [ **World Wide Web 服務**] [  >  **應用程式開發] 功能**。</span><span class="sxs-lookup"><span data-stu-id="d049d-443">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="d049d-444">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-444">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="d049d-445">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="d049d-445">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="d049d-446">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="d049d-446">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="d049d-448">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="d049d-448">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="d049d-449">在主控系統上安裝 .NET Core 裝載套件組合\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-449">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="d049d-450">套件組合會安裝 .NET Core 執行時間、.NET Core 程式庫和[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="d049d-450">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="d049d-451">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="d049d-451">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d049d-452">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="d049d-452">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="d049d-453">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-453">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="d049d-454">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="d049d-454">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="d049d-455">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="d049d-455">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="d049d-456">直接下載 (目前版本)</span><span class="sxs-lookup"><span data-stu-id="d049d-456">Direct download (current version)</span></span>

<span data-ttu-id="d049d-457">使用下列連結下載安裝程式：</span><span class="sxs-lookup"><span data-stu-id="d049d-457">Download the installer using the following link:</span></span>

[<span data-ttu-id="d049d-458">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="d049d-458">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="d049d-459">安裝程式的先前版本</span><span class="sxs-lookup"><span data-stu-id="d049d-459">Earlier versions of the installer</span></span>

<span data-ttu-id="d049d-460">若要取得安裝程式的先前版本：</span><span class="sxs-lookup"><span data-stu-id="d049d-460">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="d049d-461">流覽至 [[下載 .Net Core](https://dotnet.microsoft.com/download/dotnet-core) ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d049d-461">Navigate to the [Download .NET Core](https://dotnet.microsoft.com/download/dotnet-core) page.</span></span>
1. <span data-ttu-id="d049d-462">選取所需的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="d049d-462">Select the desired .NET Core version.</span></span>
1. <span data-ttu-id="d049d-463">在 [執行應用程式 - 執行階段]\*\*\*\* 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="d049d-463">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="d049d-464">使用**裝載**套件組合連結來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-464">Download the installer using the **Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="d049d-465">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="d049d-465">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="d049d-466">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="d049d-466">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="d049d-467">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="d049d-467">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="d049d-468">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-468">Run the installer on the server.</span></span> <span data-ttu-id="d049d-469">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="d049d-469">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="d049d-470">`OPT_NO_ANCM=1`：略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="d049d-470">`OPT_NO_ANCM=1`: Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="d049d-471">`OPT_NO_RUNTIME=1`：略過安裝 .NET Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="d049d-471">`OPT_NO_RUNTIME=1`: Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="d049d-472">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="d049d-472">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="d049d-473">`OPT_NO_SHAREDFX=1`：略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="d049d-473">`OPT_NO_SHAREDFX=1`: Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="d049d-474">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="d049d-474">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="d049d-475">`OPT_NO_X86=1`：略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="d049d-475">`OPT_NO_X86=1`: Skip installing x86 runtimes.</span></span> <span data-ttu-id="d049d-476">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="d049d-476">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="d049d-477">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="d049d-477">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="d049d-478">`OPT_NO_SHARED_CONFIG_CHECK=1`：當共用設定（*applicationhost.config*）位於與 iis 安裝相同的電腦上時，請停用 [檢查使用 iis 共用設定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-478">`OPT_NO_SHARED_CONFIG_CHECK=1`: Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="d049d-479">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="d049d-479">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="d049d-480">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="d049d-480">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="d049d-481">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d049d-481">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="d049d-482">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="d049d-482">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="d049d-483">ASP.NET Core 不採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="d049d-483">ASP.NET Core doesn't adopt roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="d049d-484">藉由安裝新的裝載套件組合來升級共用架構之後，請重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d049d-484">After upgrading the shared framework by installing a new hosting bundle, restart the system or execute the following commands in a command shell:</span></span>

```console
net stop was /y
net start w3svc
```

> [!NOTE]
> <span data-ttu-id="d049d-485">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d049d-485">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="d049d-486">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d049d-486">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="d049d-487">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="d049d-487">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="d049d-488">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-488">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="d049d-489">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="d049d-489">The preferred method is to use WebPI.</span></span> <span data-ttu-id="d049d-490">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="d049d-490">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="d049d-491">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="d049d-491">Create the IIS site</span></span>

1. <span data-ttu-id="d049d-492">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-492">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="d049d-493">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="d049d-493">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="d049d-494">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="d049d-494">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="d049d-495">在 [IIS 管理員] 中 **，在 [** 連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-495">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="d049d-496">以滑鼠右鍵按一下 [網站]\*\*\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-496">Right-click the **Sites** folder.</span></span> <span data-ttu-id="d049d-497">從操作功能表選取 [新增網站]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-497">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="d049d-498">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-498">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="d049d-499">藉由**Binding**選取 **[確定]** 來提供系結設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="d049d-499">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="d049d-501">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="d049d-501">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="d049d-502">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="d049d-502">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="d049d-503">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="d049d-503">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="d049d-504">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="d049d-504">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="d049d-505">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="d049d-505">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="d049d-506">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="d049d-506">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="d049d-507">在伺服器的節點之下，選取 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-507">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="d049d-508">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-508">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="d049d-509">在 [編輯應用程式集區]\*\*\*\* 視窗中，將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="d049d-509">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="d049d-511">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="d049d-511">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="d049d-512">ASP.NET Core 不依賴載入桌面 CLR （.NET CLR）。</span><span class="sxs-lookup"><span data-stu-id="d049d-512">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR).</span></span> <span data-ttu-id="d049d-513">.NET Core 的核心通用語言執行時間（CoreCLR）會啟動以在背景工作進程中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-513">The Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="d049d-514">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\* 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="d049d-514">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="d049d-515">*ASP.NET Core 2.2 或更新版本*：</span><span class="sxs-lookup"><span data-stu-id="d049d-515">*ASP.NET Core 2.2 or later*:</span></span>

   * <span data-ttu-id="d049d-516">針對以使用同[進程裝載模型](#in-process-hosting-model)的32位 SDK 發行的32位（x86）獨立式[部署](/dotnet/core/deploying/#self-contained-deployments-scd)，請啟用32位的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-516">For a 32-bit (x86) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) published with a 32-bit SDK that uses the [in-process hosting model](#in-process-hosting-model), enable the Application Pool for 32-bit.</span></span> <span data-ttu-id="d049d-517">在 IIS 管理員中，流覽至 [**連接**] 提要欄位中的 [**應用程式**集區]。</span><span class="sxs-lookup"><span data-stu-id="d049d-517">In IIS Manager, navigate to **Application Pools** in the **Connections** sidebar.</span></span> <span data-ttu-id="d049d-518">選取應用程式的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-518">Select the app's Application Pool.</span></span> <span data-ttu-id="d049d-519">在 [**動作**] 提要欄位中，選取 [ **Advanced Settings**]。</span><span class="sxs-lookup"><span data-stu-id="d049d-519">In the **Actions** sidebar, select **Advanced Settings**.</span></span> <span data-ttu-id="d049d-520">將 [**啟用32位應用程式**] 設定為 `True` 。</span><span class="sxs-lookup"><span data-stu-id="d049d-520">Set **Enable 32-Bit Applications** to `True`.</span></span> 

   * <span data-ttu-id="d049d-521">對於使用[同處理序主控模型](#in-process-hosting-model)的 64 位元 (x64) [自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-521">For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span> <span data-ttu-id="d049d-522">在 IIS 管理員中，流覽至 [**連接**] 提要欄位中的 [**應用程式**集區]。</span><span class="sxs-lookup"><span data-stu-id="d049d-522">In IIS Manager, navigate to **Application Pools** in the **Connections** sidebar.</span></span> <span data-ttu-id="d049d-523">選取應用程式的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-523">Select the app's Application Pool.</span></span> <span data-ttu-id="d049d-524">在 [**動作**] 提要欄位中，選取 [ **Advanced Settings**]。</span><span class="sxs-lookup"><span data-stu-id="d049d-524">In the **Actions** sidebar, select **Advanced Settings**.</span></span> <span data-ttu-id="d049d-525">將 [**啟用32位應用程式**] 設定為 `False` 。</span><span class="sxs-lookup"><span data-stu-id="d049d-525">Set **Enable 32-Bit Applications** to `False`.</span></span> 

1. <span data-ttu-id="d049d-526">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-526">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="d049d-527">如果應用程式集區的預設識別（**進程模型**  >  **Identity** ）從**ApplicationPoolIdentity**變更為另一個身分識別，請確認新的身分識別具有存取應用程式資料夾、資料庫和其他必要資源的必要許可權。</span><span class="sxs-lookup"><span data-stu-id="d049d-527">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="d049d-528">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-528">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="d049d-529">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-529">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="d049d-530">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-530">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="d049d-531">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="d049d-531">Deploy the app</span></span>

<span data-ttu-id="d049d-532">將應用程式部署至在[建立 IIS 網站](#create-the-iis-site)一節中所建立的 IIS **實體路徑**資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-532">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="d049d-533">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish]\*\* 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-533">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="d049d-534">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d049d-534">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="d049d-535">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="d049d-535">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="d049d-536">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行]\*\*\*\* 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="d049d-536">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="d049d-538">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d049d-538">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="d049d-539">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="d049d-539">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="d049d-540">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="d049d-540">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="d049d-541">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="d049d-541">Alternatives to Web Deploy</span></span>

<span data-ttu-id="d049d-542">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="d049d-542">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="d049d-543">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="d049d-543">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="d049d-544">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="d049d-544">Browse the website</span></span>

<span data-ttu-id="d049d-545">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="d049d-545">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="d049d-546">在下列範例中，網站繫結至 IIS 上的**連接埠** `80`，其**主機名稱**為 `www.mysite.com`。</span><span class="sxs-lookup"><span data-stu-id="d049d-546">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="d049d-547">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="d049d-547">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="d049d-549">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="d049d-549">Locked deployment files</span></span>

<span data-ttu-id="d049d-550">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-550">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="d049d-551">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-551">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="d049d-552">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="d049d-552">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="d049d-553">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="d049d-553">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="d049d-554">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="d049d-554">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="d049d-555">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-555">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="d049d-556">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="d049d-556">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="d049d-557">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-557">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="d049d-558">使用 PowerShell 卸載*app_offline .htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="d049d-558">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="d049d-559">資料保護</span><span class="sxs-lookup"><span data-stu-id="d049d-559">Data protection</span></span>

<span data-ttu-id="d049d-560">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d049d-560">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="d049d-561">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="d049d-561">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="d049d-562">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="d049d-562">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="d049d-563">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="d049d-563">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="d049d-564">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="d049d-564">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="d049d-565">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="d049d-565">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="d049d-566">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="d049d-566">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="d049d-567">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="d049d-567">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="d049d-568">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="d049d-568">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="d049d-569">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="d049d-569">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="d049d-570">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="d049d-570">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="d049d-571">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="d049d-571">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="d049d-572">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="d049d-572">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="d049d-573">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="d049d-573">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="d049d-574">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="d049d-574">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="d049d-575">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="d049d-575">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="d049d-576">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="d049d-576">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="d049d-577">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="d049d-577">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="d049d-578">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="d049d-578">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="d049d-579">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="d049d-579">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="d049d-580">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="d049d-580">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="d049d-581">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="d049d-581">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="d049d-582">此設定位在應用程式集區 [進階設定]\*\*\*\* 下的 [處理序模型]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="d049d-582">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="d049d-583">將 [載入使用者設定檔]\*\*\*\* 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="d049d-583">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="d049d-584">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="d049d-584">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="d049d-585">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-585">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="d049d-586">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="d049d-586">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="d049d-587">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d049d-587">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="d049d-588">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="d049d-588">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="d049d-589">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="d049d-589">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="d049d-590">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-590">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="d049d-591">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-591">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="d049d-592">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="d049d-592">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="d049d-593">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d049d-593">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="d049d-594">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="d049d-594">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="d049d-595">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="d049d-595">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="d049d-596">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="d049d-596">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="d049d-597">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="d049d-597">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="d049d-598">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="d049d-598">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="d049d-599">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="d049d-599">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="d049d-600">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="d049d-600">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="d049d-601">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="d049d-601">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="d049d-602">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="d049d-602">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="d049d-603">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-603">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="d049d-604">如需詳細資訊，請參閱<xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="d049d-604">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="d049d-605">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="d049d-605">Virtual Directories</span></span>

<span data-ttu-id="d049d-606">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="d049d-606">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="d049d-607">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="d049d-607">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="d049d-608">子應用程式</span><span class="sxs-lookup"><span data-stu-id="d049d-608">Sub-applications</span></span>

<span data-ttu-id="d049d-609">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="d049d-609">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="d049d-610">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="d049d-610">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="d049d-611">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="d049d-611">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="d049d-612">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="d049d-612">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="d049d-613">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="d049d-613">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="d049d-614">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="d049d-614">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="d049d-615">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="d049d-615">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="d049d-616">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="d049d-616">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="d049d-617">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」\*\* 回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="d049d-617">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="d049d-618">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="d049d-618">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="d049d-619">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-619">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="d049d-620">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="d049d-620">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="d049d-621">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="d049d-621">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="d049d-622">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-622">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="d049d-623">在 [新增應用程式]\*\*\*\* 對話方塊中，使用 [應用程式集區]\*\*\*\* 的[選取]\*\*\*\* 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-623">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="d049d-624">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-624">Select **OK**.</span></span>

<span data-ttu-id="d049d-625">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="d049d-625">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="d049d-626">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 。</span><span class="sxs-lookup"><span data-stu-id="d049d-626">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="d049d-627">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="d049d-627">Configuration of IIS with web.config</span></span>

<span data-ttu-id="d049d-628">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="d049d-628">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="d049d-629">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="d049d-629">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="d049d-630">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="d049d-630">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="d049d-631">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="d049d-631">For more information, see the following topics:</span></span>

* [<span data-ttu-id="d049d-632">的設定參考\<system.webServer></span><span class="sxs-lookup"><span data-stu-id="d049d-632">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="d049d-633">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數（支援 IIS 10.0 或更新版本），請參閱 IIS 參考檔中[環境變數 \<environmentVariables> ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)主題的*appcmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="d049d-633">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="d049d-634">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="d049d-634">Configuration sections of web.config</span></span>

<span data-ttu-id="d049d-635">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="d049d-635">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="d049d-636">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-636">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="d049d-637">如需詳細資訊，請參閱[Configuration](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="d049d-637">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="d049d-638">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="d049d-638">Application Pools</span></span>

<span data-ttu-id="d049d-639">應用程式集區隔離取決於裝載模型：</span><span class="sxs-lookup"><span data-stu-id="d049d-639">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="d049d-640">同進程裝載：應用程式必須在不同的應用程式集區中執行。</span><span class="sxs-lookup"><span data-stu-id="d049d-640">In-process hosting: Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="d049d-641">跨進程裝載：我們建議您在自己的應用程式集區中執行每個應用程式，以將應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="d049d-641">Out-of-process hosting: We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="d049d-642">IIS [新增網站]\*\*\*\* 對話方塊預設每個應用程式皆為單一應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-642">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="d049d-643">當提供**網站名稱**時，文字會自動轉移至 [應用程式集區]\*\*\*\* 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-643">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="d049d-644">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-644">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="d049d-645">應用程式集區Identity</span><span class="sxs-lookup"><span data-stu-id="d049d-645">Application Pool Identity</span></span>

<span data-ttu-id="d049d-646">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="d049d-646">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="d049d-647">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="d049d-647">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="d049d-648">在 IIS 管理主控台中，于應用程式集區的 [**高級設定**] 底下，確定 **Identity** 已設定為使用**ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="d049d-648">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="d049d-650">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="d049d-650">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="d049d-651">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="d049d-651">Resources can be secured using this identity.</span></span> <span data-ttu-id="d049d-652">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="d049d-652">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="d049d-653">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="d049d-653">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="d049d-654">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="d049d-654">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="d049d-655">以滑鼠右鍵按一下目錄並選取 [屬性]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-655">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="d049d-656">依序選取 [安全性]\*\*\*\* 索引標籤下的 [編輯]\*\*\*\* 按鈕和 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d049d-656">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="d049d-657">選取 [位置]\*\*\*\* 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="d049d-657">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="d049d-658">在 [輸入要選取的物件名稱]\*\*\*\* 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="d049d-658">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="d049d-659">選取 [檢查名稱]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d049d-659">Select the **Check Names** button.</span></span> <span data-ttu-id="d049d-660">針對 [DefaultAppPool]\*\*，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="d049d-660">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="d049d-661">選取 [檢查名稱]\*\*\*\* 按鈕時，[DefaultAppPool]\*\*\*\* 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="d049d-661">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="d049d-662">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="d049d-662">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="d049d-663">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="d049d-663">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="d049d-665">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-665">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="d049d-667">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-667">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="d049d-668">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-668">Provide additional permissions as needed.</span></span>

<span data-ttu-id="d049d-669">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-669">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="d049d-670">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="d049d-670">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="d049d-671">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-671">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="d049d-672">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="d049d-672">HTTP/2 support</span></span>

<span data-ttu-id="d049d-673">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="d049d-673">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="d049d-674">內含式</span><span class="sxs-lookup"><span data-stu-id="d049d-674">In-process</span></span>
  * <span data-ttu-id="d049d-675">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-675">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="d049d-676">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="d049d-676">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="d049d-677">跨處理序</span><span class="sxs-lookup"><span data-stu-id="d049d-677">Out-of-process</span></span>
  * <span data-ttu-id="d049d-678">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-678">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="d049d-679">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d049d-679">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="d049d-680">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="d049d-680">TLS 1.2 or later connection</span></span>

<span data-ttu-id="d049d-681">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="d049d-681">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="d049d-682">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="d049d-682">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="d049d-683">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="d049d-683">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="d049d-684">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="d049d-684">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="d049d-685">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d049d-685">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="d049d-686">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="d049d-686">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="d049d-687">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="d049d-687">CORS preflight requests</span></span>

<span data-ttu-id="d049d-688">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="d049d-688">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="d049d-689">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-689">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="d049d-690">若要瞭解如何在 web.config 中設定應用程式的 IIS 處理*程式來傳遞*選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="d049d-690">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="d049d-691">應用程式初始化模組與閒置逾時</span><span class="sxs-lookup"><span data-stu-id="d049d-691">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="d049d-692">在 IIS 中由 ASP.NET Core 模組版本 2 裝載時：</span><span class="sxs-lookup"><span data-stu-id="d049d-692">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="d049d-693">[應用程式初始化模組](#application-initialization-module)：應用程式裝載的同[進程](#in-process-hosting-model)或跨[進程](#out-of-process-hosting-model)可以設定為在背景工作進程重新開機或伺服器重新開機時自動啟動。</span><span class="sxs-lookup"><span data-stu-id="d049d-693">[Application Initialization Module](#application-initialization-module): App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="d049d-694">[閒置超時](#idle-timeout)：應用程式的裝載同[進程](#in-process-hosting-model)可以設定為不在非活動期間的時間超時。</span><span class="sxs-lookup"><span data-stu-id="d049d-694">[Idle Timeout](#idle-timeout): App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="d049d-695">應用程式初始化模組</span><span class="sxs-lookup"><span data-stu-id="d049d-695">Application Initialization Module</span></span>

<span data-ttu-id="d049d-696">*套用到應用程式裝載同處理序與非同處理序。*</span><span class="sxs-lookup"><span data-stu-id="d049d-696">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="d049d-697">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-697">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="d049d-698">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="d049d-698">The request triggers the app to start.</span></span> <span data-ttu-id="d049d-699">根據預設，IIS 會發出應用程式根 URL (`/`) 的要求以初始化應用程式 (如需有關設定的詳細資訊，請參閱[額外資源](#application-initialization-module-and-idle-timeout-additional-resources))。</span><span class="sxs-lookup"><span data-stu-id="d049d-699">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="d049d-700">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="d049d-700">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="d049d-701">在 Windows 7 或更新的電腦系統上，當在本機使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="d049d-701">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="d049d-702">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="d049d-702">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="d049d-703">開啟 [網際網路資訊服務]\*\* [World Wide Web 服務]\*\* > \*\* [應用程式開發功能]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-703">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="d049d-704">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-704">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="d049d-705">在 Windows Server 2008 R2 或更新版本上：</span><span class="sxs-lookup"><span data-stu-id="d049d-705">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="d049d-706">開啟「新增角色與功能精靈」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-706">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="d049d-707">在 [選取角色服務]\*\*\*\* 面板中，開啟 [應用程式開發]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-707">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="d049d-708">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-708">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="d049d-709">使用下列任一方式為網站啟用應用程式初始化模組：</span><span class="sxs-lookup"><span data-stu-id="d049d-709">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="d049d-710">使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="d049d-710">Using IIS Manager:</span></span>

  1. <span data-ttu-id="d049d-711">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-711">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="d049d-712">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-712">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="d049d-713">預設的 [啟動模式]\*\*\*\* 是 [OnDemand]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-713">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="d049d-714">將 [啟動模式]\*\*\*\* 設定為 [AlwaysRunning]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-714">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="d049d-715">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-715">Select **OK**.</span></span>
  1. <span data-ttu-id="d049d-716">開啟 [連線]\*\*\*\* 面板中的 [站台]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-716">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="d049d-717">以滑鼠右鍵按一下應用程式，然後選取 [管理網站]\*\* [進階設定]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-717">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="d049d-718">預設 [預先載入已啟用]\*\*\*\* 設定是 [False]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-718">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="d049d-719">將 [預先載入已啟用]\*\*\*\* 設定為 [True]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-719">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="d049d-720">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-720">Select **OK**.</span></span>

* <span data-ttu-id="d049d-721">使用 *web.config*，新增 `<applicationInitialization>` 元素並將 `doAppInitAfterRestart` 設定為 `true` 至應用程式 *web.config* 檔案中的 `<system.webServer>` 元素：</span><span class="sxs-lookup"><span data-stu-id="d049d-721">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <applicationInitialization doAppInitAfterRestart="true" />
      </system.webServer>
    </location>
  </configuration>
  ```

### <a name="idle-timeout"></a><span data-ttu-id="d049d-722">閒置逾時</span><span class="sxs-lookup"><span data-stu-id="d049d-722">Idle Timeout</span></span>

<span data-ttu-id="d049d-723">*僅適用於應用程式裝載同處理序。*</span><span class="sxs-lookup"><span data-stu-id="d049d-723">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="d049d-724">若要防止應用程式閒置，請使用 IIS 管理員設定應用程式集區的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="d049d-724">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="d049d-725">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-725">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="d049d-726">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-726">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="d049d-727">預設 [閒置逾時 (分鐘)]\*\*\*\* 是 **20** 分鐘。</span><span class="sxs-lookup"><span data-stu-id="d049d-727">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="d049d-728">將 [閒置逾時 (分鐘)]\*\*\*\* 設定為 **0** (零)。</span><span class="sxs-lookup"><span data-stu-id="d049d-728">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="d049d-729">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-729">Select **OK**.</span></span>
1. <span data-ttu-id="d049d-730">回收背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="d049d-730">Recycle the worker process.</span></span>

<span data-ttu-id="d049d-731">若要防止應用程式裝載[非同處理序](#out-of-process-hosting-model)逾時，請使用下列任一方式：</span><span class="sxs-lookup"><span data-stu-id="d049d-731">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="d049d-732">從外部服務對應用程式執行 Ping 以讓它繼續執行。</span><span class="sxs-lookup"><span data-stu-id="d049d-732">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="d049d-733">若應用程式只裝載背景服務，避免 IIS 裝載並使用 [Windows 服務來裝載 ASP.NET Core 應用程式](xref:host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="d049d-733">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="d049d-734">應用程式初始化模組與閒置逾時額外資源</span><span class="sxs-lookup"><span data-stu-id="d049d-734">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="d049d-735">IIS 8.0 應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="d049d-735">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="d049d-736">[應用程式 \<applicationInitialization> 初始化](/iis/configuration/system.webserver/applicationinitialization/)。</span><span class="sxs-lookup"><span data-stu-id="d049d-736">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="d049d-737">[應用程式集 \<processModel> 區的進程模型設定](/iis/configuration/system.applicationhost/applicationpools/add/processmodel)。</span><span class="sxs-lookup"><span data-stu-id="d049d-737">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="d049d-738">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="d049d-738">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="d049d-739">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="d049d-739">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="d049d-740">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="d049d-740">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="d049d-741">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="d049d-741">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="d049d-742">其他資源</span><span class="sxs-lookup"><span data-stu-id="d049d-742">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="d049d-743">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="d049d-743">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="d049d-744">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="d049d-744">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="d049d-745">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="d049d-745">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="d049d-746">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="d049d-746">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="d049d-747">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="d049d-747">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="d049d-748">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="d049d-748">Supported operating systems</span></span>

<span data-ttu-id="d049d-749">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="d049d-749">The following operating systems are supported:</span></span>

* <span data-ttu-id="d049d-750">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-750">Windows 7 or later</span></span>
* <span data-ttu-id="d049d-751">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-751">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="d049d-752">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-752">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="d049d-753">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="d049d-753">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="d049d-754">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="d049d-754">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="d049d-755">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="d049d-755">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="d049d-756">支援的平台</span><span class="sxs-lookup"><span data-stu-id="d049d-756">Supported platforms</span></span>

<span data-ttu-id="d049d-757">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-757">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="d049d-758">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="d049d-758">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="d049d-759">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="d049d-759">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="d049d-760">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="d049d-760">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="d049d-761">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="d049d-761">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="d049d-762">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-762">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="d049d-763">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="d049d-763">A 64-bit runtime must be present on the host system.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="d049d-764">裝載模型</span><span class="sxs-lookup"><span data-stu-id="d049d-764">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="d049d-765">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="d049d-765">In-process hosting model</span></span>

<span data-ttu-id="d049d-766">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="d049d-766">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="d049d-767">同進程裝載可提供跨進程裝載的效能提升，原因如下：</span><span class="sxs-lookup"><span data-stu-id="d049d-767">In-process hosting provides improved performance over out-of-process hosting because:</span></span>

* <span data-ttu-id="d049d-768">要求不會透過回送介面卡進行 proxy 處理。</span><span class="sxs-lookup"><span data-stu-id="d049d-768">Requests aren't proxied over the loopback adapter.</span></span> <span data-ttu-id="d049d-769">回送介面卡是一種網路介面，可將連出的網路流量傳回給同一部電腦。</span><span class="sxs-lookup"><span data-stu-id="d049d-769">A loopback adapter is a network interface that returns outgoing network traffic back to the same machine.</span></span>

<span data-ttu-id="d049d-770">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="d049d-770">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="d049d-771">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)：</span><span class="sxs-lookup"><span data-stu-id="d049d-771">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="d049d-772">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="d049d-772">Performs app initialization.</span></span>
  * <span data-ttu-id="d049d-773">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="d049d-773">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="d049d-774">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="d049d-774">Calls `Program.Main`.</span></span>
* <span data-ttu-id="d049d-775">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="d049d-775">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="d049d-776">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="d049d-776">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="d049d-777">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="d049d-777">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-inprocess.png)

<span data-ttu-id="d049d-779">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-779">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="d049d-780">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="d049d-780">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="d049d-781">ASP.NET Core 模組會接收原生要求，並將它傳遞至 IIS HTTP 伺服器（ `IISHttpServer` ）。</span><span class="sxs-lookup"><span data-stu-id="d049d-781">The ASP.NET Core Module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="d049d-782">IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。</span><span class="sxs-lookup"><span data-stu-id="d049d-782">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="d049d-783">IIS HTTP 伺服器處理要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="d049d-783">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="d049d-784">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d049d-784">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="d049d-785">應用程式的回應會透過 IIS HTTP 伺服器傳回 IIS。</span><span class="sxs-lookup"><span data-stu-id="d049d-785">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="d049d-786">IIS 會將回應傳送到起始該要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="d049d-786">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="d049d-787">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="d049d-787">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="d049d-788">`CreateDefaultBuilder` 會透過呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 方法來啟動 [CoreCLR](/dotnet/standard/glossary#coreclr) 以新增 <xref:Microsoft.AspNetCore.Hosting.Server.IServer>，並在 IIS 工作者處理序 (*w3wp.exe* 或 *iisexpress.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-788">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="d049d-789">效能測試指出，相較於跨處理序裝載 .NET Core 應用程式，並將要求 Proxy 處理至 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器，裝載於同處理序可提供明顯更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="d049d-789">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="d049d-790">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="d049d-790">Out-of-process hosting model</span></span>

<span data-ttu-id="d049d-791">因為 ASP.NET Core 應用程式會在與 IIS 背景工作進程不同的進程中執行，所以 ASP.NET Core 模組會處理進程管理。</span><span class="sxs-lookup"><span data-stu-id="d049d-791">Because ASP.NET Core apps run in a process separate from the IIS worker process, the ASP.NET Core Module handles process management.</span></span> <span data-ttu-id="d049d-792">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="d049d-792">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="d049d-793">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="d049d-793">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="d049d-794">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="d049d-794">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![非同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-outofprocess.png)

<span data-ttu-id="d049d-796">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-796">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="d049d-797">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="d049d-797">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="d049d-798">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="d049d-798">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="d049d-799">此模組在啟動時透過環境變數指定連接埠，而 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 延伸模組則會設定伺服器來接聽 `http://localhost:{PORT}`。</span><span class="sxs-lookup"><span data-stu-id="d049d-799">The module specifies the port via an environment variable at startup, and the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="d049d-800">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="d049d-800">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="d049d-801">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="d049d-801">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="d049d-802">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="d049d-802">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="d049d-803">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d049d-803">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="d049d-804">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d049d-804">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="d049d-805">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="d049d-805">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="d049d-806">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="d049d-806">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="d049d-807">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="d049d-807">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="d049d-808">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="d049d-808">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="d049d-809">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="d049d-809">Enable the IISIntegration components</span></span>

<span data-ttu-id="d049d-810">在 `CreateWebHostBuilder` （*Program.cs*）中建立主機時，請呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="d049d-810">When building a host in `CreateWebHostBuilder` (*Program.cs*), call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="d049d-811">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host>。</span><span class="sxs-lookup"><span data-stu-id="d049d-811">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="d049d-812">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="d049d-812">IIS options</span></span>

<span data-ttu-id="d049d-813">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="d049d-813">**In-process hosting model**</span></span>

<span data-ttu-id="d049d-814">若要設定 IIS 伺服器選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISServerOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-814">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="d049d-815">下列範例會停用 AutomaticAuthentication：</span><span class="sxs-lookup"><span data-stu-id="d049d-815">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="d049d-816">選項</span><span class="sxs-lookup"><span data-stu-id="d049d-816">Option</span></span>                         | <span data-ttu-id="d049d-817">預設</span><span class="sxs-lookup"><span data-stu-id="d049d-817">Default</span></span> | <span data-ttu-id="d049d-818">設定</span><span class="sxs-lookup"><span data-stu-id="d049d-818">Setting</span></span> |
| ---
<span data-ttu-id="d049d-819">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-819">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-820">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-820">'Blazor'</span></span>
- <span data-ttu-id="d049d-821">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-821">'Identity'</span></span>
- <span data-ttu-id="d049d-822">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-822">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-823">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-823">'Razor'</span></span>
- <span data-ttu-id="d049d-824">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-824">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-825">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-825">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-826">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-826">'Blazor'</span></span>
- <span data-ttu-id="d049d-827">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-827">'Identity'</span></span>
- <span data-ttu-id="d049d-828">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-828">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-829">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-829">'Razor'</span></span>
- <span data-ttu-id="d049d-830">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-830">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-831">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-831">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-832">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-832">'Blazor'</span></span>
- <span data-ttu-id="d049d-833">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-833">'Identity'</span></span>
- <span data-ttu-id="d049d-834">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-834">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-835">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-835">'Razor'</span></span>
- <span data-ttu-id="d049d-836">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-836">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-837">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-837">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-838">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-838">'Blazor'</span></span>
- <span data-ttu-id="d049d-839">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-839">'Identity'</span></span>
- <span data-ttu-id="d049d-840">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-840">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-841">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-841">'Razor'</span></span>
- <span data-ttu-id="d049d-842">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-842">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-843">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-843">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-844">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-844">'Blazor'</span></span>
- <span data-ttu-id="d049d-845">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-845">'Identity'</span></span>
- <span data-ttu-id="d049d-846">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-846">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-847">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-847">'Razor'</span></span>
- <span data-ttu-id="d049d-848">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-848">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-849">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-849">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-850">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-850">'Blazor'</span></span>
- <span data-ttu-id="d049d-851">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-851">'Identity'</span></span>
- <span data-ttu-id="d049d-852">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-852">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-853">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-853">'Razor'</span></span>
- <span data-ttu-id="d049d-854">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-854">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-855">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-855">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-856">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-856">'Blazor'</span></span>
- <span data-ttu-id="d049d-857">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-857">'Identity'</span></span>
- <span data-ttu-id="d049d-858">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-858">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-859">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-859">'Razor'</span></span>
- <span data-ttu-id="d049d-860">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-860">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-861">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-861">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-862">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-862">'Blazor'</span></span>
- <span data-ttu-id="d049d-863">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-863">'Identity'</span></span>
- <span data-ttu-id="d049d-864">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-864">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-865">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-865">'Razor'</span></span>
- <span data-ttu-id="d049d-866">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-866">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-867">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-867">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-868">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-868">'Blazor'</span></span>
- <span data-ttu-id="d049d-869">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-869">'Identity'</span></span>
- <span data-ttu-id="d049d-870">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-870">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-871">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-871">'Razor'</span></span>
- <span data-ttu-id="d049d-872">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-872">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-873">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-873">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-874">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-874">'Blazor'</span></span>
- <span data-ttu-id="d049d-875">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-875">'Identity'</span></span>
- <span data-ttu-id="d049d-876">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-876">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-877">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-877">'Razor'</span></span>
- <span data-ttu-id="d049d-878">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-878">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-879">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-879">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-880">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-880">'Blazor'</span></span>
- <span data-ttu-id="d049d-881">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-881">'Identity'</span></span>
- <span data-ttu-id="d049d-882">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-882">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-883">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-883">'Razor'</span></span>
- <span data-ttu-id="d049d-884">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-884">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-885">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-885">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-886">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-886">'Blazor'</span></span>
- <span data-ttu-id="d049d-887">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-887">'Identity'</span></span>
- <span data-ttu-id="d049d-888">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-888">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-889">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-889">'Razor'</span></span>
- <span data-ttu-id="d049d-890">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-890">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-891">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-891">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-892">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-892">'Blazor'</span></span>
- <span data-ttu-id="d049d-893">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-893">'Identity'</span></span>
- <span data-ttu-id="d049d-894">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-894">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-895">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-895">'Razor'</span></span>
- <span data-ttu-id="d049d-896">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-896">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-897">--------------- |:-----: |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-897">--------------- | :-----: | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-898">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-898">'Blazor'</span></span>
- <span data-ttu-id="d049d-899">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-899">'Identity'</span></span>
- <span data-ttu-id="d049d-900">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-900">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-901">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-901">'Razor'</span></span>
- <span data-ttu-id="d049d-902">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-902">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-903">---- | |`AutomaticAuthentication`      | `true` |若 `true` 為，IIS 伺服器會設定 `HttpContext.User` 由[Windows 驗證](xref:security/authentication/windowsauth)所驗證的。</span><span class="sxs-lookup"><span data-stu-id="d049d-903">---- | | `AutomaticAuthentication`      | `true`  | If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="d049d-904">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="d049d-904">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="d049d-905">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="d049d-905">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="d049d-906">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="d049d-906">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="d049d-907">| |`AuthenticationDisplayName`    | `null` |設定使用者在登入頁面上顯示的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="d049d-907">| | `AuthenticationDisplayName`    | `null`  | Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="d049d-908">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="d049d-908">**Out-of-process hosting model**</span></span>

<span data-ttu-id="d049d-909">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-909">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="d049d-910">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="d049d-910">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="d049d-911">選項</span><span class="sxs-lookup"><span data-stu-id="d049d-911">Option</span></span>                         | <span data-ttu-id="d049d-912">預設</span><span class="sxs-lookup"><span data-stu-id="d049d-912">Default</span></span> | <span data-ttu-id="d049d-913">設定</span><span class="sxs-lookup"><span data-stu-id="d049d-913">Setting</span></span> |
| ---
<span data-ttu-id="d049d-914">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-914">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-915">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-915">'Blazor'</span></span>
- <span data-ttu-id="d049d-916">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-916">'Identity'</span></span>
- <span data-ttu-id="d049d-917">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-917">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-918">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-918">'Razor'</span></span>
- <span data-ttu-id="d049d-919">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-919">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-920">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-920">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-921">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-921">'Blazor'</span></span>
- <span data-ttu-id="d049d-922">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-922">'Identity'</span></span>
- <span data-ttu-id="d049d-923">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-923">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-924">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-924">'Razor'</span></span>
- <span data-ttu-id="d049d-925">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-925">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-926">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-926">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-927">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-927">'Blazor'</span></span>
- <span data-ttu-id="d049d-928">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-928">'Identity'</span></span>
- <span data-ttu-id="d049d-929">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-929">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-930">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-930">'Razor'</span></span>
- <span data-ttu-id="d049d-931">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-931">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-932">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-932">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-933">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-933">'Blazor'</span></span>
- <span data-ttu-id="d049d-934">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-934">'Identity'</span></span>
- <span data-ttu-id="d049d-935">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-935">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-936">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-936">'Razor'</span></span>
- <span data-ttu-id="d049d-937">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-937">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-938">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-938">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-939">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-939">'Blazor'</span></span>
- <span data-ttu-id="d049d-940">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-940">'Identity'</span></span>
- <span data-ttu-id="d049d-941">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-941">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-942">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-942">'Razor'</span></span>
- <span data-ttu-id="d049d-943">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-943">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-944">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-944">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-945">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-945">'Blazor'</span></span>
- <span data-ttu-id="d049d-946">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-946">'Identity'</span></span>
- <span data-ttu-id="d049d-947">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-947">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-948">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-948">'Razor'</span></span>
- <span data-ttu-id="d049d-949">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-949">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-950">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-950">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-951">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-951">'Blazor'</span></span>
- <span data-ttu-id="d049d-952">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-952">'Identity'</span></span>
- <span data-ttu-id="d049d-953">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-953">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-954">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-954">'Razor'</span></span>
- <span data-ttu-id="d049d-955">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-955">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-956">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-956">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-957">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-957">'Blazor'</span></span>
- <span data-ttu-id="d049d-958">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-958">'Identity'</span></span>
- <span data-ttu-id="d049d-959">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-959">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-960">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-960">'Razor'</span></span>
- <span data-ttu-id="d049d-961">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-961">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-962">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-962">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-963">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-963">'Blazor'</span></span>
- <span data-ttu-id="d049d-964">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-964">'Identity'</span></span>
- <span data-ttu-id="d049d-965">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-965">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-966">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-966">'Razor'</span></span>
- <span data-ttu-id="d049d-967">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-967">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-968">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-968">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-969">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-969">'Blazor'</span></span>
- <span data-ttu-id="d049d-970">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-970">'Identity'</span></span>
- <span data-ttu-id="d049d-971">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-971">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-972">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-972">'Razor'</span></span>
- <span data-ttu-id="d049d-973">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-973">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-974">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-974">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-975">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-975">'Blazor'</span></span>
- <span data-ttu-id="d049d-976">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-976">'Identity'</span></span>
- <span data-ttu-id="d049d-977">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-977">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-978">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-978">'Razor'</span></span>
- <span data-ttu-id="d049d-979">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-979">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-980">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-980">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-981">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-981">'Blazor'</span></span>
- <span data-ttu-id="d049d-982">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-982">'Identity'</span></span>
- <span data-ttu-id="d049d-983">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-983">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-984">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-984">'Razor'</span></span>
- <span data-ttu-id="d049d-985">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-985">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-986">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-986">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-987">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-987">'Blazor'</span></span>
- <span data-ttu-id="d049d-988">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-988">'Identity'</span></span>
- <span data-ttu-id="d049d-989">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-989">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-990">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-990">'Razor'</span></span>
- <span data-ttu-id="d049d-991">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-991">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-992">--------------- |:-----: |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-992">--------------- | :-----: | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-993">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-993">'Blazor'</span></span>
- <span data-ttu-id="d049d-994">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-994">'Identity'</span></span>
- <span data-ttu-id="d049d-995">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-995">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-996">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-996">'Razor'</span></span>
- <span data-ttu-id="d049d-997">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-997">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-998">---- | |`AutomaticAuthentication`      | `true` |若 `true` 為， [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定 `HttpContext.User` 由[Windows 驗證](xref:security/authentication/windowsauth)所驗證的。</span><span class="sxs-lookup"><span data-stu-id="d049d-998">---- | | `AutomaticAuthentication`      | `true`  | If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="d049d-999">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="d049d-999">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="d049d-1000">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1000">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="d049d-1001">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-1001">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> <span data-ttu-id="d049d-1002">| |`AuthenticationDisplayName`    | `null` |設定使用者在登入頁面上顯示的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="d049d-1002">| | `AuthenticationDisplayName`    | `null`  | Sets the display name shown to users on login pages.</span></span> <span data-ttu-id="d049d-1003">| |`ForwardClientCertificate`     | `true` |如果 `true` 和 `MS-ASPNETCORE-CLIENTCERT` 要求標頭存在，則 `HttpContext.Connection.ClientCertificate` 會填入。</span><span class="sxs-lookup"><span data-stu-id="d049d-1003">| | `ForwardClientCertificate`     | `true`  | If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d049d-1004">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="d049d-1004">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d049d-1005">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="d049d-1005">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="d049d-1006">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-1006">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="d049d-1007">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1007">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="d049d-1008">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="d049d-1008">web.config file</span></span>

<span data-ttu-id="d049d-1009">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1009">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="d049d-1010">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="d049d-1010">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="d049d-1011">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1011">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="d049d-1012">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="d049d-1012">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="d049d-1013">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 ASP.NET Core 模組，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1013">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="d049d-1014">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="d049d-1014">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="d049d-1015">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-1015">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="d049d-1016">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-1016">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="d049d-1017">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-1017">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="d049d-1018">若要防止 Web SDK*轉換 web.config 檔案*，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="d049d-1018">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="d049d-1019">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1019">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="d049d-1020">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1020">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="d049d-1021">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="d049d-1021">web.config file location</span></span>

<span data-ttu-id="d049d-1022">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) *，web.config 檔案*必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="d049d-1022">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="d049d-1023">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="d049d-1023">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="d049d-1024">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1024">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="d049d-1025">機密檔案存在於應用程式的實體路徑，例如\* \<assembly> .runtimeconfig.json. json*、 \* \<assembly> .xml* （xml 檔批註）和\* \<assembly> .deps.json\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1025">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="d049d-1026">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="d049d-1026">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="d049d-1027">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1027">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="d049d-1028">\***Web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署*移除 web.config 檔案\*。**</span><span class="sxs-lookup"><span data-stu-id="d049d-1028">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="d049d-1029">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="d049d-1029">Transform web.config</span></span>

<span data-ttu-id="d049d-1030">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1030">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="d049d-1031">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="d049d-1031">IIS configuration</span></span>

<span data-ttu-id="d049d-1032">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="d049d-1032">**Windows Server operating systems**</span></span>

<span data-ttu-id="d049d-1033">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="d049d-1033">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="d049d-1034">使用來自 [管理]\*\*\*\* 功能表的 [新增角色及功能]\*\*\*\* 精靈，或是 [伺服器管理員]\*\*\*\* 中的連結。</span><span class="sxs-lookup"><span data-stu-id="d049d-1034">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="d049d-1035">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-1035">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="d049d-1037">在 [功能]\*\*\*\* 步驟之後，[角色服務]\*\*\*\* 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="d049d-1037">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="d049d-1038">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="d049d-1038">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="d049d-1040">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-1040">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="d049d-1041">若要啟用 Windows 驗證，請展開下列節點： [**網頁伺服器**  >  **安全性**]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1041">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="d049d-1042">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-1042">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="d049d-1043">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1043">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="d049d-1044">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-1044">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="d049d-1045">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="d049d-1045">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="d049d-1046">若要啟用 websocket，請展開下列節點： [**網頁伺服器**  >  **應用程式開發**]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1046">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="d049d-1047">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-1047">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="d049d-1048">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1048">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="d049d-1049">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="d049d-1049">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="d049d-1050">安裝**網頁伺服器（iis）** 角色之後，不需要重新開機伺服器/iis。</span><span class="sxs-lookup"><span data-stu-id="d049d-1050">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="d049d-1051">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="d049d-1051">**Windows desktop operating systems**</span></span>

<span data-ttu-id="d049d-1052">啟用 [IIS 管理主控台]\*\*\*\* 和 [World Wide Web 服務]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1052">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="d049d-1053">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1053">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="d049d-1054">開啟 [Internet Information Services]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-1054">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="d049d-1055">開啟 [Web 管理工具]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-1055">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="d049d-1056">核取 [IIS 管理主控台]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-1056">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="d049d-1057">[World Wide Web Services] (全球資訊網服務)\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-1057">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="d049d-1058">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-1058">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="d049d-1059">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-1059">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="d049d-1060">若要啟用 Windows 驗證，請展開下列節點： **World Wide Web 服務**  >  **安全性**。</span><span class="sxs-lookup"><span data-stu-id="d049d-1060">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="d049d-1061">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-1061">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="d049d-1062">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1062">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="d049d-1063">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-1063">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="d049d-1064">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="d049d-1064">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="d049d-1065">若要啟用 websocket，請展開下列節點： [ **World Wide Web 服務**] [  >  **應用程式開發] 功能**。</span><span class="sxs-lookup"><span data-stu-id="d049d-1065">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="d049d-1066">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-1066">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="d049d-1067">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1067">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="d049d-1068">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="d049d-1068">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="d049d-1070">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="d049d-1070">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="d049d-1071">在主控系統上安裝 .NET Core 裝載套件組合\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1071">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="d049d-1072">套件組合會安裝 .NET Core 執行時間、.NET Core 程式庫和[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1072">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="d049d-1073">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="d049d-1073">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d049d-1074">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="d049d-1074">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="d049d-1075">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1075">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="d049d-1076">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="d049d-1076">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="d049d-1077">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1077">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="download"></a><span data-ttu-id="d049d-1078">下載</span><span class="sxs-lookup"><span data-stu-id="d049d-1078">Download</span></span>

1. <span data-ttu-id="d049d-1079">流覽至 [[下載 .Net Core](https://dotnet.microsoft.com/download/dotnet-core) ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d049d-1079">Navigate to the [Download .NET Core](https://dotnet.microsoft.com/download/dotnet-core) page.</span></span>
1. <span data-ttu-id="d049d-1080">選取所需的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="d049d-1080">Select the desired .NET Core version.</span></span>
1. <span data-ttu-id="d049d-1081">在 [執行應用程式 - 執行階段]\*\*\*\* 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="d049d-1081">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="d049d-1082">使用**裝載**套件組合連結來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1082">Download the installer using the **Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="d049d-1083">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="d049d-1083">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="d049d-1084">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1084">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="d049d-1085">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="d049d-1085">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="d049d-1086">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1086">Run the installer on the server.</span></span> <span data-ttu-id="d049d-1087">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="d049d-1087">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="d049d-1088">`OPT_NO_ANCM=1`：略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="d049d-1088">`OPT_NO_ANCM=1`: Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="d049d-1089">`OPT_NO_RUNTIME=1`：略過安裝 .NET Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="d049d-1089">`OPT_NO_RUNTIME=1`: Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="d049d-1090">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1090">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="d049d-1091">`OPT_NO_SHAREDFX=1`：略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="d049d-1091">`OPT_NO_SHAREDFX=1`: Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="d049d-1092">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1092">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="d049d-1093">`OPT_NO_X86=1`：略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="d049d-1093">`OPT_NO_X86=1`: Skip installing x86 runtimes.</span></span> <span data-ttu-id="d049d-1094">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="d049d-1094">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="d049d-1095">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="d049d-1095">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="d049d-1096">`OPT_NO_SHARED_CONFIG_CHECK=1`：當共用設定（*applicationhost.config*）位於與 iis 安裝相同的電腦上時，請停用 [檢查使用 iis 共用設定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1096">`OPT_NO_SHARED_CONFIG_CHECK=1`: Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="d049d-1097">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="d049d-1097">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="d049d-1098">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1098">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="d049d-1099">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d049d-1099">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="d049d-1100">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="d049d-1100">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="d049d-1101">安裝裝載套件組合時，不需要手動停止 IIS 中的個別網站。</span><span class="sxs-lookup"><span data-stu-id="d049d-1101">It isn't necessary to manually stop individual sites in IIS when installing the Hosting Bundle.</span></span> <span data-ttu-id="d049d-1102">裝載的應用程式（IIS 網站）會在 IIS 重新開機時重新開機。</span><span class="sxs-lookup"><span data-stu-id="d049d-1102">Hosted apps (IIS sites) restart when IIS restarts.</span></span> <span data-ttu-id="d049d-1103">應用程式會在收到第一個要求時重新開機，包括從[應用程式初始化模組](#application-initialization-module-and-idle-timeout)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1103">Apps start up again when they receive their first request, including from the [Application Initialization Module](#application-initialization-module-and-idle-timeout).</span></span>

<span data-ttu-id="d049d-1104">ASP.NET Core 採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="d049d-1104">ASP.NET Core adopts roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="d049d-1105">當 IIS 所裝載的應用程式使用 IIS 重新開機時，應用程式會在收到第一個要求時，以其所參考套件的最新修補程式版本來載入。</span><span class="sxs-lookup"><span data-stu-id="d049d-1105">When apps hosted by IIS restart with IIS, the apps load with the latest patch releases of their referenced packages when they receive their first request.</span></span> <span data-ttu-id="d049d-1106">如果未重新開機 IIS，應用程式會在其工作者進程回收時重新開機並展示向前復原行為，並接收其第一個要求。</span><span class="sxs-lookup"><span data-stu-id="d049d-1106">If IIS isn't restarted, apps restart and exhibit roll-forward behavior when their worker processes are recycled and they receive their first request.</span></span>

> [!NOTE]
> <span data-ttu-id="d049d-1107">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1107">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="d049d-1108">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d049d-1108">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="d049d-1109">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="d049d-1109">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="d049d-1110">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1110">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="d049d-1111">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="d049d-1111">The preferred method is to use WebPI.</span></span> <span data-ttu-id="d049d-1112">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="d049d-1112">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="d049d-1113">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="d049d-1113">Create the IIS site</span></span>

1. <span data-ttu-id="d049d-1114">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1114">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="d049d-1115">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="d049d-1115">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="d049d-1116">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1116">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="d049d-1117">在 [IIS 管理員] 中 **，在 [** 連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-1117">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="d049d-1118">以滑鼠右鍵按一下 [網站]\*\*\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1118">Right-click the **Sites** folder.</span></span> <span data-ttu-id="d049d-1119">從操作功能表選取 [新增網站]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1119">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="d049d-1120">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1120">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="d049d-1121">藉由**Binding**選取 **[確定]** 來提供系結設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="d049d-1121">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="d049d-1123">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1123">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="d049d-1124">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="d049d-1124">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="d049d-1125">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1125">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="d049d-1126">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="d049d-1126">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="d049d-1127">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="d049d-1127">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="d049d-1128">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1128">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="d049d-1129">在伺服器的節點之下，選取 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1129">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="d049d-1130">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1130">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="d049d-1131">在 [編輯應用程式集區]\*\*\*\* 視窗中，將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="d049d-1131">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="d049d-1133">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="d049d-1133">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="d049d-1134">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1134">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="d049d-1135">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\* 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="d049d-1135">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="d049d-1136">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](#in-process-hosting-model)的 64 位元 (x64) [獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1136">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="d049d-1137">在 IIS 管理員的 [動作]\*\*\*\* 資訊看板 > [應用程式集區]\*\*\*\* 中，選取 [設定應用程式集區預設值]\*\*\*\* 或 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1137">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="d049d-1138">找到 [啟用 32 位元應用程式]\*\*\*\*，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1138">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="d049d-1139">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1139">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="d049d-1140">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-1140">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="d049d-1141">如果應用程式集區的預設識別（**進程模型**  >  **Identity** ）從**ApplicationPoolIdentity**變更為另一個身分識別，請確認新的身分識別具有存取應用程式資料夾、資料庫和其他必要資源的必要許可權。</span><span class="sxs-lookup"><span data-stu-id="d049d-1141">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="d049d-1142">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-1142">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="d049d-1143">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-1143">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="d049d-1144">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-1144">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="d049d-1145">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="d049d-1145">Deploy the app</span></span>

<span data-ttu-id="d049d-1146">將應用程式部署至在[建立 IIS 網站](#create-the-iis-site)一節中所建立的 IIS **實體路徑**資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1146">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="d049d-1147">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish]\*\* 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1147">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="d049d-1148">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d049d-1148">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="d049d-1149">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="d049d-1149">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="d049d-1150">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行]\*\*\*\* 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="d049d-1150">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="d049d-1152">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d049d-1152">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="d049d-1153">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1153">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="d049d-1154">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1154">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="d049d-1155">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="d049d-1155">Alternatives to Web Deploy</span></span>

<span data-ttu-id="d049d-1156">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1156">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="d049d-1157">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="d049d-1157">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="d049d-1158">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="d049d-1158">Browse the website</span></span>

<span data-ttu-id="d049d-1159">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="d049d-1159">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="d049d-1160">在下列範例中，網站繫結至 IIS 上的**連接埠** `80`，其**主機名稱**為 `www.mysite.com`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1160">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="d049d-1161">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="d049d-1161">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="d049d-1163">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="d049d-1163">Locked deployment files</span></span>

<span data-ttu-id="d049d-1164">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1164">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="d049d-1165">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1165">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="d049d-1166">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="d049d-1166">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="d049d-1167">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1167">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="d049d-1168">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="d049d-1168">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="d049d-1169">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1169">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="d049d-1170">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1170">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="d049d-1171">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1171">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="d049d-1172">使用 PowerShell 卸載*app_offline .htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="d049d-1172">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="d049d-1173">資料保護</span><span class="sxs-lookup"><span data-stu-id="d049d-1173">Data protection</span></span>

<span data-ttu-id="d049d-1174">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d049d-1174">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="d049d-1175">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1175">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="d049d-1176">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="d049d-1176">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="d049d-1177">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="d049d-1177">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="d049d-1178">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="d049d-1178">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="d049d-1179">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="d049d-1179">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="d049d-1180">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="d049d-1180">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="d049d-1181">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1181">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="d049d-1182">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="d049d-1182">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="d049d-1183">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="d049d-1183">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="d049d-1184">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="d049d-1184">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="d049d-1185">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="d049d-1185">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="d049d-1186">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1186">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="d049d-1187">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="d049d-1187">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="d049d-1188">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="d049d-1188">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="d049d-1189">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="d049d-1189">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="d049d-1190">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="d049d-1190">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="d049d-1191">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="d049d-1191">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="d049d-1192">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="d049d-1192">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="d049d-1193">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="d049d-1193">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="d049d-1194">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1194">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="d049d-1195">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="d049d-1195">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="d049d-1196">此設定位在應用程式集區 [進階設定]\*\*\*\* 下的 [處理序模型]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="d049d-1196">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="d049d-1197">將 [載入使用者設定檔]\*\*\*\* 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1197">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="d049d-1198">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="d049d-1198">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="d049d-1199">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1199">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="d049d-1200">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1200">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="d049d-1201">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1201">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="d049d-1202">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1202">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="d049d-1203">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="d049d-1203">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="d049d-1204">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1204">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="d049d-1205">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1205">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="d049d-1206">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="d049d-1206">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="d049d-1207">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1207">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="d049d-1208">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="d049d-1208">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="d049d-1209">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1209">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="d049d-1210">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="d049d-1210">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="d049d-1211">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1211">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="d049d-1212">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="d049d-1212">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="d049d-1213">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1213">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="d049d-1214">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="d049d-1214">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="d049d-1215">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1215">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="d049d-1216">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="d049d-1216">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="d049d-1217">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-1217">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="d049d-1218">如需詳細資訊，請參閱<xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1218">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="d049d-1219">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="d049d-1219">Virtual Directories</span></span>

<span data-ttu-id="d049d-1220">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1220">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="d049d-1221">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="d049d-1221">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="d049d-1222">子應用程式</span><span class="sxs-lookup"><span data-stu-id="d049d-1222">Sub-applications</span></span>

<span data-ttu-id="d049d-1223">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="d049d-1223">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="d049d-1224">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="d049d-1224">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="d049d-1225">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="d049d-1225">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="d049d-1226">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="d049d-1226">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="d049d-1227">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1227">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="d049d-1228">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="d049d-1228">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="d049d-1229">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="d049d-1229">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="d049d-1230">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="d049d-1230">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="d049d-1231">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」\*\* 回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1231">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="d049d-1232">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="d049d-1232">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="d049d-1233">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1233">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="d049d-1234">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="d049d-1234">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="d049d-1235">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="d049d-1235">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="d049d-1236">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1236">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="d049d-1237">在 [新增應用程式]\*\*\*\* 對話方塊中，使用 [應用程式集區]\*\*\*\* 的[選取]\*\*\*\* 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1237">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="d049d-1238">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1238">Select **OK**.</span></span>

<span data-ttu-id="d049d-1239">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="d049d-1239">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="d049d-1240">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 。</span><span class="sxs-lookup"><span data-stu-id="d049d-1240">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="d049d-1241">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="d049d-1241">Configuration of IIS with web.config</span></span>

<span data-ttu-id="d049d-1242">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="d049d-1242">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="d049d-1243">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1243">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="d049d-1244">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1244">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="d049d-1245">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="d049d-1245">For more information, see the following topics:</span></span>

* [<span data-ttu-id="d049d-1246">的設定參考\<system.webServer></span><span class="sxs-lookup"><span data-stu-id="d049d-1246">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="d049d-1247">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數（支援 IIS 10.0 或更新版本），請參閱 IIS 參考檔中[環境變數 \<environmentVariables> ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)主題的*appcmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="d049d-1247">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="d049d-1248">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="d049d-1248">Configuration sections of web.config</span></span>

<span data-ttu-id="d049d-1249">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="d049d-1249">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="d049d-1250">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1250">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="d049d-1251">如需詳細資訊，請參閱[Configuration](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1251">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="d049d-1252">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="d049d-1252">Application Pools</span></span>

<span data-ttu-id="d049d-1253">應用程式集區隔離取決於裝載模型：</span><span class="sxs-lookup"><span data-stu-id="d049d-1253">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="d049d-1254">同進程裝載：應用程式必須在不同的應用程式集區中執行。</span><span class="sxs-lookup"><span data-stu-id="d049d-1254">In-process hosting: Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="d049d-1255">跨進程裝載：我們建議您在自己的應用程式集區中執行每個應用程式，以將應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="d049d-1255">Out-of-process hosting: We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="d049d-1256">IIS [新增網站]\*\*\*\* 對話方塊預設每個應用程式皆為單一應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1256">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="d049d-1257">當提供**網站名稱**時，文字會自動轉移至 [應用程式集區]\*\*\*\* 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-1257">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="d049d-1258">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1258">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="d049d-1259">應用程式集區Identity</span><span class="sxs-lookup"><span data-stu-id="d049d-1259">Application Pool Identity</span></span>

<span data-ttu-id="d049d-1260">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="d049d-1260">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="d049d-1261">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="d049d-1261">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="d049d-1262">在 IIS 管理主控台中，于應用程式集區的 [**高級設定**] 底下，確定 **Identity** 已設定為使用**ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="d049d-1262">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="d049d-1264">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="d049d-1264">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="d049d-1265">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="d049d-1265">Resources can be secured using this identity.</span></span> <span data-ttu-id="d049d-1266">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="d049d-1266">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="d049d-1267">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="d049d-1267">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="d049d-1268">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="d049d-1268">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="d049d-1269">以滑鼠右鍵按一下目錄並選取 [屬性]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1269">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="d049d-1270">依序選取 [安全性]\*\*\*\* 索引標籤下的 [編輯]\*\*\*\* 按鈕和 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d049d-1270">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="d049d-1271">選取 [位置]\*\*\*\* 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="d049d-1271">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="d049d-1272">在 [輸入要選取的物件名稱]\*\*\*\* 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="d049d-1272">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="d049d-1273">選取 [檢查名稱]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d049d-1273">Select the **Check Names** button.</span></span> <span data-ttu-id="d049d-1274">針對 [DefaultAppPool]\*\*，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="d049d-1274">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="d049d-1275">選取 [檢查名稱]\*\*\*\* 按鈕時，[DefaultAppPool]\*\*\*\* 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="d049d-1275">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="d049d-1276">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="d049d-1276">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="d049d-1277">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1277">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="d049d-1279">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1279">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="d049d-1281">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-1281">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="d049d-1282">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-1282">Provide additional permissions as needed.</span></span>

<span data-ttu-id="d049d-1283">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-1283">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="d049d-1284">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="d049d-1284">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="d049d-1285">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-1285">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="d049d-1286">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="d049d-1286">HTTP/2 support</span></span>

<span data-ttu-id="d049d-1287">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="d049d-1287">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="d049d-1288">內含式</span><span class="sxs-lookup"><span data-stu-id="d049d-1288">In-process</span></span>
  * <span data-ttu-id="d049d-1289">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-1289">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="d049d-1290">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="d049d-1290">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="d049d-1291">跨處理序</span><span class="sxs-lookup"><span data-stu-id="d049d-1291">Out-of-process</span></span>
  * <span data-ttu-id="d049d-1292">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-1292">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="d049d-1293">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d049d-1293">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="d049d-1294">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="d049d-1294">TLS 1.2 or later connection</span></span>

<span data-ttu-id="d049d-1295">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1295">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="d049d-1296">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1296">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="d049d-1297">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1297">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="d049d-1298">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1298">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="d049d-1299">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d049d-1299">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="d049d-1300">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1300">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="d049d-1301">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="d049d-1301">CORS preflight requests</span></span>

<span data-ttu-id="d049d-1302">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="d049d-1302">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="d049d-1303">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1303">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="d049d-1304">若要瞭解如何在 web.config 中設定應用程式的 IIS 處理*程式來傳遞*選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1304">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="d049d-1305">應用程式初始化模組與閒置逾時</span><span class="sxs-lookup"><span data-stu-id="d049d-1305">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="d049d-1306">在 IIS 中由 ASP.NET Core 模組版本 2 裝載時：</span><span class="sxs-lookup"><span data-stu-id="d049d-1306">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="d049d-1307">[應用程式初始化模組](#application-initialization-module)：應用程式裝載的同[進程](#in-process-hosting-model)或跨[進程](#out-of-process-hosting-model)可以設定為在背景工作進程重新開機或伺服器重新開機時自動啟動。</span><span class="sxs-lookup"><span data-stu-id="d049d-1307">[Application Initialization Module](#application-initialization-module): App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="d049d-1308">[閒置超時](#idle-timeout)：應用程式的裝載同[進程](#in-process-hosting-model)可以設定為不在非活動期間的時間超時。</span><span class="sxs-lookup"><span data-stu-id="d049d-1308">[Idle Timeout](#idle-timeout): App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="d049d-1309">應用程式初始化模組</span><span class="sxs-lookup"><span data-stu-id="d049d-1309">Application Initialization Module</span></span>

<span data-ttu-id="d049d-1310">*套用到應用程式裝載同處理序與非同處理序。*</span><span class="sxs-lookup"><span data-stu-id="d049d-1310">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="d049d-1311">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1311">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="d049d-1312">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="d049d-1312">The request triggers the app to start.</span></span> <span data-ttu-id="d049d-1313">根據預設，IIS 會發出應用程式根 URL (`/`) 的要求以初始化應用程式 (如需有關設定的詳細資訊，請參閱[額外資源](#application-initialization-module-and-idle-timeout-additional-resources))。</span><span class="sxs-lookup"><span data-stu-id="d049d-1313">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="d049d-1314">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="d049d-1314">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="d049d-1315">在 Windows 7 或更新的電腦系統上，當在本機使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="d049d-1315">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="d049d-1316">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1316">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="d049d-1317">開啟 [網際網路資訊服務]\*\* [World Wide Web 服務]\*\* > \*\* [應用程式開發功能]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1317">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="d049d-1318">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-1318">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="d049d-1319">在 Windows Server 2008 R2 或更新版本上：</span><span class="sxs-lookup"><span data-stu-id="d049d-1319">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="d049d-1320">開啟「新增角色與功能精靈」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1320">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="d049d-1321">在 [選取角色服務]\*\*\*\* 面板中，開啟 [應用程式開發]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-1321">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="d049d-1322">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-1322">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="d049d-1323">使用下列任一方式為網站啟用應用程式初始化模組：</span><span class="sxs-lookup"><span data-stu-id="d049d-1323">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="d049d-1324">使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="d049d-1324">Using IIS Manager:</span></span>

  1. <span data-ttu-id="d049d-1325">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1325">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="d049d-1326">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1326">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="d049d-1327">預設的 [啟動模式]\*\*\*\* 是 [OnDemand]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1327">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="d049d-1328">將 [啟動模式]\*\*\*\* 設定為 [AlwaysRunning]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1328">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="d049d-1329">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1329">Select **OK**.</span></span>
  1. <span data-ttu-id="d049d-1330">開啟 [連線]\*\*\*\* 面板中的 [站台]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-1330">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="d049d-1331">以滑鼠右鍵按一下應用程式，然後選取 [管理網站]\*\* [進階設定]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1331">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="d049d-1332">預設 [預先載入已啟用]\*\*\*\* 設定是 [False]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1332">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="d049d-1333">將 [預先載入已啟用]\*\*\*\* 設定為 [True]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1333">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="d049d-1334">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1334">Select **OK**.</span></span>

* <span data-ttu-id="d049d-1335">使用 *web.config*，新增 `<applicationInitialization>` 元素並將 `doAppInitAfterRestart` 設定為 `true` 至應用程式 *web.config* 檔案中的 `<system.webServer>` 元素：</span><span class="sxs-lookup"><span data-stu-id="d049d-1335">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <applicationInitialization doAppInitAfterRestart="true" />
      </system.webServer>
    </location>
  </configuration>
  ```

### <a name="idle-timeout"></a><span data-ttu-id="d049d-1336">閒置逾時</span><span class="sxs-lookup"><span data-stu-id="d049d-1336">Idle Timeout</span></span>

<span data-ttu-id="d049d-1337">*僅適用於應用程式裝載同處理序。*</span><span class="sxs-lookup"><span data-stu-id="d049d-1337">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="d049d-1338">若要防止應用程式閒置，請使用 IIS 管理員設定應用程式集區的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="d049d-1338">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="d049d-1339">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1339">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="d049d-1340">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1340">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="d049d-1341">預設 [閒置逾時 (分鐘)]\*\*\*\* 是 **20** 分鐘。</span><span class="sxs-lookup"><span data-stu-id="d049d-1341">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="d049d-1342">將 [閒置逾時 (分鐘)]\*\*\*\* 設定為 **0** (零)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1342">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="d049d-1343">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1343">Select **OK**.</span></span>
1. <span data-ttu-id="d049d-1344">回收背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="d049d-1344">Recycle the worker process.</span></span>

<span data-ttu-id="d049d-1345">若要防止應用程式裝載[非同處理序](#out-of-process-hosting-model)逾時，請使用下列任一方式：</span><span class="sxs-lookup"><span data-stu-id="d049d-1345">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="d049d-1346">從外部服務對應用程式執行 Ping 以讓它繼續執行。</span><span class="sxs-lookup"><span data-stu-id="d049d-1346">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="d049d-1347">若應用程式只裝載背景服務，避免 IIS 裝載並使用 [Windows 服務來裝載 ASP.NET Core 應用程式](xref:host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1347">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="d049d-1348">應用程式初始化模組與閒置逾時額外資源</span><span class="sxs-lookup"><span data-stu-id="d049d-1348">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="d049d-1349">IIS 8.0 應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="d049d-1349">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="d049d-1350">[應用程式 \<applicationInitialization> 初始化](/iis/configuration/system.webserver/applicationinitialization/)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1350">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="d049d-1351">[應用程式集 \<processModel> 區的進程模型設定](/iis/configuration/system.applicationhost/applicationpools/add/processmodel)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1351">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="d049d-1352">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="d049d-1352">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="d049d-1353">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="d049d-1353">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="d049d-1354">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="d049d-1354">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="d049d-1355">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="d049d-1355">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="d049d-1356">其他資源</span><span class="sxs-lookup"><span data-stu-id="d049d-1356">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="d049d-1357">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="d049d-1357">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="d049d-1358">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="d049d-1358">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="d049d-1359">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="d049d-1359">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d049d-1360">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1360">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="d049d-1361">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="d049d-1361">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="d049d-1362">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="d049d-1362">Supported operating systems</span></span>

<span data-ttu-id="d049d-1363">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="d049d-1363">The following operating systems are supported:</span></span>

* <span data-ttu-id="d049d-1364">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-1364">Windows 7 or later</span></span>
* <span data-ttu-id="d049d-1365">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-1365">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="d049d-1366">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-1366">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="d049d-1367">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1367">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="d049d-1368">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1368">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="d049d-1369">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1369">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="d049d-1370">支援的平台</span><span class="sxs-lookup"><span data-stu-id="d049d-1370">Supported platforms</span></span>

<span data-ttu-id="d049d-1371">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1371">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="d049d-1372">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="d049d-1372">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="d049d-1373">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="d049d-1373">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="d049d-1374">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="d049d-1374">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="d049d-1375">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="d049d-1375">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="d049d-1376">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1376">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="d049d-1377">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="d049d-1377">A 64-bit runtime must be present on the host system.</span></span>

<span data-ttu-id="d049d-1378">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，其為預設的跨平台 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="d049d-1378">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), a default, cross-platform HTTP server.</span></span>

<span data-ttu-id="d049d-1379">在使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，應用程式會執行於從 IIS 背景工作處理序中分離出的處理序 (跨處理序\*\*)，並搭配 [Kestrel 伺服器](xref:fundamentals/servers/index#kestrel)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1379">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](xref:fundamentals/servers/index#kestrel).</span></span>

<span data-ttu-id="d049d-1380">因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="d049d-1380">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="d049d-1381">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="d049d-1381">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="d049d-1382">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="d049d-1382">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="d049d-1383">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="d049d-1383">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core 模組](index/_static/ancm-outofprocess.png)

<span data-ttu-id="d049d-1385">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1385">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="d049d-1386">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1386">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="d049d-1387">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="d049d-1387">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="d049d-1388">此模組會在啟動時透過環境變數指定埠，而[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)會設定伺服器來接聽 `http://localhost:{port}` 。</span><span class="sxs-lookup"><span data-stu-id="d049d-1388">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="d049d-1389">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="d049d-1389">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="d049d-1390">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="d049d-1390">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="d049d-1391">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="d049d-1391">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="d049d-1392">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d049d-1392">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="d049d-1393">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d049d-1393">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="d049d-1394">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="d049d-1394">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="d049d-1395">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設為網頁伺服器，並設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)的基底路徑與連接埠來啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="d049d-1395">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="d049d-1396">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="d049d-1396">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="d049d-1397">`CreateDefaultBuilder` 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 方法。</span><span class="sxs-lookup"><span data-stu-id="d049d-1397">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="d049d-1398">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`127.0.0.1`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="d049d-1398">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="d049d-1399">若動態連接埠是 1234，Kestrel 會在 `127.0.0.1:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="d049d-1399">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="d049d-1400">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="d049d-1400">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="d049d-1401">Kestrel 的接聽 API</span><span class="sxs-lookup"><span data-stu-id="d049d-1401">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="d049d-1402">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="d049d-1402">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="d049d-1403">在使用模組時不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="d049d-1403">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="d049d-1404">若呼叫 `UseUrls` 或 `Listen`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="d049d-1404">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="d049d-1405">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1405">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="d049d-1406">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1406">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="d049d-1407">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="d049d-1407">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="d049d-1408">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="d049d-1408">Enable the IISIntegration components</span></span>

<span data-ttu-id="d049d-1409">在 `CreateWebHostBuilder` （*Program.cs*）中建立主機時，請呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="d049d-1409">When building a host in `CreateWebHostBuilder` (*Program.cs*), call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="d049d-1410">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1410">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="d049d-1411">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="d049d-1411">IIS options</span></span>

| <span data-ttu-id="d049d-1412">選項</span><span class="sxs-lookup"><span data-stu-id="d049d-1412">Option</span></span>                         | <span data-ttu-id="d049d-1413">預設</span><span class="sxs-lookup"><span data-stu-id="d049d-1413">Default</span></span> | <span data-ttu-id="d049d-1414">設定</span><span class="sxs-lookup"><span data-stu-id="d049d-1414">Setting</span></span> |
| ---
<span data-ttu-id="d049d-1415">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1415">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1416">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1416">'Blazor'</span></span>
- <span data-ttu-id="d049d-1417">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1417">'Identity'</span></span>
- <span data-ttu-id="d049d-1418">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1418">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1419">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1419">'Razor'</span></span>
- <span data-ttu-id="d049d-1420">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1420">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1421">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1421">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1422">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1422">'Blazor'</span></span>
- <span data-ttu-id="d049d-1423">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1423">'Identity'</span></span>
- <span data-ttu-id="d049d-1424">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1424">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1425">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1425">'Razor'</span></span>
- <span data-ttu-id="d049d-1426">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1426">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1427">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1427">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1428">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1428">'Blazor'</span></span>
- <span data-ttu-id="d049d-1429">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1429">'Identity'</span></span>
- <span data-ttu-id="d049d-1430">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1430">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1431">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1431">'Razor'</span></span>
- <span data-ttu-id="d049d-1432">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1432">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1433">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1433">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1434">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1434">'Blazor'</span></span>
- <span data-ttu-id="d049d-1435">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1435">'Identity'</span></span>
- <span data-ttu-id="d049d-1436">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1436">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1437">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1437">'Razor'</span></span>
- <span data-ttu-id="d049d-1438">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1438">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1439">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1439">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1440">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1440">'Blazor'</span></span>
- <span data-ttu-id="d049d-1441">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1441">'Identity'</span></span>
- <span data-ttu-id="d049d-1442">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1442">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1443">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1443">'Razor'</span></span>
- <span data-ttu-id="d049d-1444">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1444">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1445">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1445">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1446">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1446">'Blazor'</span></span>
- <span data-ttu-id="d049d-1447">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1447">'Identity'</span></span>
- <span data-ttu-id="d049d-1448">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1448">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1449">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1449">'Razor'</span></span>
- <span data-ttu-id="d049d-1450">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1450">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1451">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1451">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1452">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1452">'Blazor'</span></span>
- <span data-ttu-id="d049d-1453">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1453">'Identity'</span></span>
- <span data-ttu-id="d049d-1454">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1454">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1455">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1455">'Razor'</span></span>
- <span data-ttu-id="d049d-1456">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1456">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1457">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1457">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1458">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1458">'Blazor'</span></span>
- <span data-ttu-id="d049d-1459">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1459">'Identity'</span></span>
- <span data-ttu-id="d049d-1460">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1460">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1461">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1461">'Razor'</span></span>
- <span data-ttu-id="d049d-1462">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1462">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1463">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1463">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1464">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1464">'Blazor'</span></span>
- <span data-ttu-id="d049d-1465">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1465">'Identity'</span></span>
- <span data-ttu-id="d049d-1466">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1466">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1467">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1467">'Razor'</span></span>
- <span data-ttu-id="d049d-1468">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1468">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1469">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1469">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1470">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1470">'Blazor'</span></span>
- <span data-ttu-id="d049d-1471">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1471">'Identity'</span></span>
- <span data-ttu-id="d049d-1472">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1472">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1473">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1473">'Razor'</span></span>
- <span data-ttu-id="d049d-1474">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1474">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1475">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1475">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1476">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1476">'Blazor'</span></span>
- <span data-ttu-id="d049d-1477">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1477">'Identity'</span></span>
- <span data-ttu-id="d049d-1478">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1478">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1479">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1479">'Razor'</span></span>
- <span data-ttu-id="d049d-1480">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1480">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1481">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1481">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1482">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1482">'Blazor'</span></span>
- <span data-ttu-id="d049d-1483">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1483">'Identity'</span></span>
- <span data-ttu-id="d049d-1484">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1484">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1485">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1485">'Razor'</span></span>
- <span data-ttu-id="d049d-1486">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1486">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1487">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1487">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1488">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1488">'Blazor'</span></span>
- <span data-ttu-id="d049d-1489">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1489">'Identity'</span></span>
- <span data-ttu-id="d049d-1490">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1490">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1491">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1491">'Razor'</span></span>
- <span data-ttu-id="d049d-1492">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1492">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-1493">--------------- |:-----: |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1493">--------------- | :-----: | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1494">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1494">'Blazor'</span></span>
- <span data-ttu-id="d049d-1495">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1495">'Identity'</span></span>
- <span data-ttu-id="d049d-1496">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1496">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1497">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1497">'Razor'</span></span>
- <span data-ttu-id="d049d-1498">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1498">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-1499">---- | |`AutomaticAuthentication`      | `true` |若 `true` 為，IIS 伺服器會設定 `HttpContext.User` 由[Windows 驗證](xref:security/authentication/windowsauth)所驗證的。</span><span class="sxs-lookup"><span data-stu-id="d049d-1499">---- | | `AutomaticAuthentication`      | `true`  | If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="d049d-1500">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="d049d-1500">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="d049d-1501">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1501">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="d049d-1502">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1502">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="d049d-1503">| |`AuthenticationDisplayName`    | `null` |設定使用者在登入頁面上顯示的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="d049d-1503">| | `AuthenticationDisplayName`    | `null`  | Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="d049d-1504">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-1504">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="d049d-1505">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="d049d-1505">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="d049d-1506">選項</span><span class="sxs-lookup"><span data-stu-id="d049d-1506">Option</span></span>                         | <span data-ttu-id="d049d-1507">預設</span><span class="sxs-lookup"><span data-stu-id="d049d-1507">Default</span></span> | <span data-ttu-id="d049d-1508">設定</span><span class="sxs-lookup"><span data-stu-id="d049d-1508">Setting</span></span> |
| ---
<span data-ttu-id="d049d-1509">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1509">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1510">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1510">'Blazor'</span></span>
- <span data-ttu-id="d049d-1511">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1511">'Identity'</span></span>
- <span data-ttu-id="d049d-1512">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1512">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1513">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1513">'Razor'</span></span>
- <span data-ttu-id="d049d-1514">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1514">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1515">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1515">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1516">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1516">'Blazor'</span></span>
- <span data-ttu-id="d049d-1517">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1517">'Identity'</span></span>
- <span data-ttu-id="d049d-1518">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1518">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1519">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1519">'Razor'</span></span>
- <span data-ttu-id="d049d-1520">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1520">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1521">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1521">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1522">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1522">'Blazor'</span></span>
- <span data-ttu-id="d049d-1523">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1523">'Identity'</span></span>
- <span data-ttu-id="d049d-1524">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1524">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1525">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1525">'Razor'</span></span>
- <span data-ttu-id="d049d-1526">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1526">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1527">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1527">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1528">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1528">'Blazor'</span></span>
- <span data-ttu-id="d049d-1529">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1529">'Identity'</span></span>
- <span data-ttu-id="d049d-1530">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1530">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1531">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1531">'Razor'</span></span>
- <span data-ttu-id="d049d-1532">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1532">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1533">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1533">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1534">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1534">'Blazor'</span></span>
- <span data-ttu-id="d049d-1535">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1535">'Identity'</span></span>
- <span data-ttu-id="d049d-1536">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1536">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1537">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1537">'Razor'</span></span>
- <span data-ttu-id="d049d-1538">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1538">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1539">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1539">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1540">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1540">'Blazor'</span></span>
- <span data-ttu-id="d049d-1541">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1541">'Identity'</span></span>
- <span data-ttu-id="d049d-1542">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1542">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1543">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1543">'Razor'</span></span>
- <span data-ttu-id="d049d-1544">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1544">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1545">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1545">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1546">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1546">'Blazor'</span></span>
- <span data-ttu-id="d049d-1547">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1547">'Identity'</span></span>
- <span data-ttu-id="d049d-1548">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1548">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1549">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1549">'Razor'</span></span>
- <span data-ttu-id="d049d-1550">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1550">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1551">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1551">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1552">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1552">'Blazor'</span></span>
- <span data-ttu-id="d049d-1553">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1553">'Identity'</span></span>
- <span data-ttu-id="d049d-1554">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1554">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1555">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1555">'Razor'</span></span>
- <span data-ttu-id="d049d-1556">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1556">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1557">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1557">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1558">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1558">'Blazor'</span></span>
- <span data-ttu-id="d049d-1559">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1559">'Identity'</span></span>
- <span data-ttu-id="d049d-1560">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1560">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1561">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1561">'Razor'</span></span>
- <span data-ttu-id="d049d-1562">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1562">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1563">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1563">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1564">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1564">'Blazor'</span></span>
- <span data-ttu-id="d049d-1565">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1565">'Identity'</span></span>
- <span data-ttu-id="d049d-1566">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1566">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1567">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1567">'Razor'</span></span>
- <span data-ttu-id="d049d-1568">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1568">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1569">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1569">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1570">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1570">'Blazor'</span></span>
- <span data-ttu-id="d049d-1571">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1571">'Identity'</span></span>
- <span data-ttu-id="d049d-1572">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1572">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1573">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1573">'Razor'</span></span>
- <span data-ttu-id="d049d-1574">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1574">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1575">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1575">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1576">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1576">'Blazor'</span></span>
- <span data-ttu-id="d049d-1577">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1577">'Identity'</span></span>
- <span data-ttu-id="d049d-1578">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1578">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1579">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1579">'Razor'</span></span>
- <span data-ttu-id="d049d-1580">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1580">'SignalR' uid:</span></span> 

-
<span data-ttu-id="d049d-1581">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1581">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1582">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1582">'Blazor'</span></span>
- <span data-ttu-id="d049d-1583">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1583">'Identity'</span></span>
- <span data-ttu-id="d049d-1584">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1584">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1585">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1585">'Razor'</span></span>
- <span data-ttu-id="d049d-1586">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1586">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-1587">--------------- |:-----: |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d049d-1587">--------------- | :-----: | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d049d-1588">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1588">'Blazor'</span></span>
- <span data-ttu-id="d049d-1589">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d049d-1589">'Identity'</span></span>
- <span data-ttu-id="d049d-1590">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d049d-1590">'Let's Encrypt'</span></span>
- <span data-ttu-id="d049d-1591">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d049d-1591">'Razor'</span></span>
- <span data-ttu-id="d049d-1592">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d049d-1592">'SignalR' uid:</span></span> 

<span data-ttu-id="d049d-1593">---- | |`AutomaticAuthentication`      | `true` |若 `true` 為， [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定 `HttpContext.User` 由[Windows 驗證](xref:security/authentication/windowsauth)所驗證的。</span><span class="sxs-lookup"><span data-stu-id="d049d-1593">---- | | `AutomaticAuthentication`      | `true`  | If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="d049d-1594">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="d049d-1594">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="d049d-1595">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1595">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="d049d-1596">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-1596">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> <span data-ttu-id="d049d-1597">| |`AuthenticationDisplayName`    | `null` |設定使用者在登入頁面上顯示的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="d049d-1597">| | `AuthenticationDisplayName`    | `null`  | Sets the display name shown to users on login pages.</span></span> <span data-ttu-id="d049d-1598">| |`ForwardClientCertificate`     | `true` |如果 `true` 和 `MS-ASPNETCORE-CLIENTCERT` 要求標頭存在，則 `HttpContext.Connection.ClientCertificate` 會填入。</span><span class="sxs-lookup"><span data-stu-id="d049d-1598">| | `ForwardClientCertificate`     | `true`  | If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d049d-1599">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="d049d-1599">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d049d-1600">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="d049d-1600">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="d049d-1601">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-1601">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="d049d-1602">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1602">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="d049d-1603">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="d049d-1603">web.config file</span></span>

<span data-ttu-id="d049d-1604">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1604">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="d049d-1605">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="d049d-1605">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="d049d-1606">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1606">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="d049d-1607">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="d049d-1607">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="d049d-1608">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 ASP.NET Core 模組，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1608">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="d049d-1609">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="d049d-1609">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="d049d-1610">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-1610">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="d049d-1611">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-1611">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="d049d-1612">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-1612">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="d049d-1613">若要防止 Web SDK*轉換 web.config 檔案*，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="d049d-1613">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="d049d-1614">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1614">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="d049d-1615">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1615">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="d049d-1616">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="d049d-1616">web.config file location</span></span>

<span data-ttu-id="d049d-1617">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) *，web.config 檔案*必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="d049d-1617">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="d049d-1618">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="d049d-1618">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="d049d-1619">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1619">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="d049d-1620">機密檔案存在於應用程式的實體路徑，例如\* \<assembly> .runtimeconfig.json. json*、 \* \<assembly> .xml* （xml 檔批註）和\* \<assembly> .deps.json\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1620">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="d049d-1621">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="d049d-1621">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="d049d-1622">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1622">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="d049d-1623">\***Web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署*移除 web.config 檔案\*。**</span><span class="sxs-lookup"><span data-stu-id="d049d-1623">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="d049d-1624">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="d049d-1624">Transform web.config</span></span>

<span data-ttu-id="d049d-1625">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1625">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="d049d-1626">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="d049d-1626">IIS configuration</span></span>

<span data-ttu-id="d049d-1627">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="d049d-1627">**Windows Server operating systems**</span></span>

<span data-ttu-id="d049d-1628">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="d049d-1628">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="d049d-1629">使用來自 [管理]\*\*\*\* 功能表的 [新增角色及功能]\*\*\*\* 精靈，或是 [伺服器管理員]\*\*\*\* 中的連結。</span><span class="sxs-lookup"><span data-stu-id="d049d-1629">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="d049d-1630">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-1630">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="d049d-1632">在 [功能]\*\*\*\* 步驟之後，[角色服務]\*\*\*\* 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="d049d-1632">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="d049d-1633">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="d049d-1633">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="d049d-1635">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-1635">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="d049d-1636">若要啟用 Windows 驗證，請展開下列節點： [**網頁伺服器**  >  **安全性**]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1636">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="d049d-1637">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-1637">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="d049d-1638">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1638">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="d049d-1639">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-1639">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="d049d-1640">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="d049d-1640">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="d049d-1641">若要啟用 websocket，請展開下列節點： [**網頁伺服器**  >  **應用程式開發**]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1641">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="d049d-1642">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-1642">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="d049d-1643">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1643">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="d049d-1644">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="d049d-1644">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="d049d-1645">安裝**網頁伺服器（iis）** 角色之後，不需要重新開機伺服器/iis。</span><span class="sxs-lookup"><span data-stu-id="d049d-1645">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="d049d-1646">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="d049d-1646">**Windows desktop operating systems**</span></span>

<span data-ttu-id="d049d-1647">啟用 [IIS 管理主控台]\*\*\*\* 和 [World Wide Web 服務]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1647">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="d049d-1648">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1648">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="d049d-1649">開啟 [Internet Information Services]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-1649">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="d049d-1650">開啟 [Web 管理工具]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-1650">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="d049d-1651">核取 [IIS 管理主控台]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-1651">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="d049d-1652">[World Wide Web Services] (全球資訊網服務)\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-1652">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="d049d-1653">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-1653">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="d049d-1654">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-1654">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="d049d-1655">若要啟用 Windows 驗證，請展開下列節點： **World Wide Web 服務**  >  **安全性**。</span><span class="sxs-lookup"><span data-stu-id="d049d-1655">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="d049d-1656">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-1656">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="d049d-1657">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1657">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="d049d-1658">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-1658">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="d049d-1659">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="d049d-1659">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="d049d-1660">若要啟用 websocket，請展開下列節點： [ **World Wide Web 服務**] [  >  **應用程式開發] 功能**。</span><span class="sxs-lookup"><span data-stu-id="d049d-1660">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="d049d-1661">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="d049d-1661">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="d049d-1662">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1662">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="d049d-1663">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="d049d-1663">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="d049d-1665">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="d049d-1665">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="d049d-1666">在主控系統上安裝 .NET Core 裝載套件組合\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1666">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="d049d-1667">套件組合會安裝 .NET Core 執行時間、.NET Core 程式庫和[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1667">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="d049d-1668">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="d049d-1668">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d049d-1669">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="d049d-1669">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="d049d-1670">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1670">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="d049d-1671">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="d049d-1671">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="d049d-1672">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1672">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="download"></a><span data-ttu-id="d049d-1673">下載</span><span class="sxs-lookup"><span data-stu-id="d049d-1673">Download</span></span>

1. <span data-ttu-id="d049d-1674">流覽至 [[下載 .Net Core](https://dotnet.microsoft.com/download/dotnet-core) ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d049d-1674">Navigate to the [Download .NET Core](https://dotnet.microsoft.com/download/dotnet-core) page.</span></span>
1. <span data-ttu-id="d049d-1675">選取所需的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="d049d-1675">Select the desired .NET Core version.</span></span>
1. <span data-ttu-id="d049d-1676">在 [執行應用程式 - 執行階段]\*\*\*\* 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="d049d-1676">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="d049d-1677">使用**裝載**套件組合連結來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1677">Download the installer using the **Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="d049d-1678">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="d049d-1678">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="d049d-1679">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1679">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="d049d-1680">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="d049d-1680">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="d049d-1681">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1681">Run the installer on the server.</span></span> <span data-ttu-id="d049d-1682">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="d049d-1682">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="d049d-1683">`OPT_NO_ANCM=1`：略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="d049d-1683">`OPT_NO_ANCM=1`: Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="d049d-1684">`OPT_NO_RUNTIME=1`：略過安裝 .NET Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="d049d-1684">`OPT_NO_RUNTIME=1`: Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="d049d-1685">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1685">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="d049d-1686">`OPT_NO_SHAREDFX=1`：略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="d049d-1686">`OPT_NO_SHAREDFX=1`: Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="d049d-1687">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1687">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="d049d-1688">`OPT_NO_X86=1`：略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="d049d-1688">`OPT_NO_X86=1`: Skip installing x86 runtimes.</span></span> <span data-ttu-id="d049d-1689">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="d049d-1689">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="d049d-1690">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="d049d-1690">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="d049d-1691">`OPT_NO_SHARED_CONFIG_CHECK=1`：當共用設定（*applicationhost.config*）位於與 iis 安裝相同的電腦上時，請停用 [檢查使用 iis 共用設定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1691">`OPT_NO_SHARED_CONFIG_CHECK=1`: Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="d049d-1692">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="d049d-1692">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="d049d-1693">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1693">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="d049d-1694">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d049d-1694">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="d049d-1695">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="d049d-1695">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="d049d-1696">安裝裝載套件組合時，不需要手動停止 IIS 中的個別網站。</span><span class="sxs-lookup"><span data-stu-id="d049d-1696">It isn't necessary to manually stop individual sites in IIS when installing the Hosting Bundle.</span></span> <span data-ttu-id="d049d-1697">裝載的應用程式（IIS 網站）會在 IIS 重新開機時重新開機。</span><span class="sxs-lookup"><span data-stu-id="d049d-1697">Hosted apps (IIS sites) restart when IIS restarts.</span></span> <span data-ttu-id="d049d-1698">應用程式會在收到第一個要求時重新開機，包括從[應用程式初始化模組](#application-initialization-module-and-idle-timeout)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1698">Apps start up again when they receive their first request, including from the [Application Initialization Module](#application-initialization-module-and-idle-timeout).</span></span>

<span data-ttu-id="d049d-1699">ASP.NET Core 採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="d049d-1699">ASP.NET Core adopts roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="d049d-1700">當 IIS 所裝載的應用程式使用 IIS 重新開機時，應用程式會在收到第一個要求時，以其所參考套件的最新修補程式版本來載入。</span><span class="sxs-lookup"><span data-stu-id="d049d-1700">When apps hosted by IIS restart with IIS, the apps load with the latest patch releases of their referenced packages when they receive their first request.</span></span> <span data-ttu-id="d049d-1701">如果未重新開機 IIS，應用程式會在其工作者進程回收時重新開機並展示向前復原行為，並接收其第一個要求。</span><span class="sxs-lookup"><span data-stu-id="d049d-1701">If IIS isn't restarted, apps restart and exhibit roll-forward behavior when their worker processes are recycled and they receive their first request.</span></span>

> [!NOTE]
> <span data-ttu-id="d049d-1702">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1702">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="d049d-1703">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d049d-1703">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="d049d-1704">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="d049d-1704">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="d049d-1705">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1705">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="d049d-1706">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="d049d-1706">The preferred method is to use WebPI.</span></span> <span data-ttu-id="d049d-1707">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="d049d-1707">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="d049d-1708">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="d049d-1708">Create the IIS site</span></span>

1. <span data-ttu-id="d049d-1709">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1709">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="d049d-1710">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="d049d-1710">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="d049d-1711">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1711">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="d049d-1712">在 [IIS 管理員] 中 **，在 [** 連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="d049d-1712">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="d049d-1713">以滑鼠右鍵按一下 [網站]\*\*\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1713">Right-click the **Sites** folder.</span></span> <span data-ttu-id="d049d-1714">從操作功能表選取 [新增網站]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1714">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="d049d-1715">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1715">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="d049d-1716">藉由**Binding**選取 **[確定]** 來提供系結設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="d049d-1716">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="d049d-1718">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1718">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="d049d-1719">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="d049d-1719">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="d049d-1720">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1720">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="d049d-1721">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="d049d-1721">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="d049d-1722">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="d049d-1722">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="d049d-1723">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1723">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="d049d-1724">在伺服器的節點之下，選取 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1724">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="d049d-1725">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1725">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="d049d-1726">在 [編輯應用程式集區]\*\*\*\* 視窗中，將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="d049d-1726">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="d049d-1728">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="d049d-1728">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="d049d-1729">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1729">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="d049d-1730">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\* 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="d049d-1730">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="d049d-1731">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](#in-process-hosting-model)的 64 位元 (x64) [獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1731">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="d049d-1732">在 IIS 管理員的 [動作]\*\*\*\* 資訊看板 > [應用程式集區]\*\*\*\* 中，選取 [設定應用程式集區預設值]\*\*\*\* 或 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1732">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="d049d-1733">找到 [啟用 32 位元應用程式]\*\*\*\*，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1733">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="d049d-1734">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1734">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="d049d-1735">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-1735">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="d049d-1736">如果應用程式集區的預設識別（**進程模型**  >  **Identity** ）從**ApplicationPoolIdentity**變更為另一個身分識別，請確認新的身分識別具有存取應用程式資料夾、資料庫和其他必要資源的必要許可權。</span><span class="sxs-lookup"><span data-stu-id="d049d-1736">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="d049d-1737">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-1737">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="d049d-1738">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="d049d-1738">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="d049d-1739">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-1739">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="d049d-1740">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="d049d-1740">Deploy the app</span></span>

<span data-ttu-id="d049d-1741">將應用程式部署至在[建立 IIS 網站](#create-the-iis-site)一節中所建立的 IIS **實體路徑**資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1741">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="d049d-1742">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish]\*\* 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1742">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="d049d-1743">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d049d-1743">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="d049d-1744">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="d049d-1744">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="d049d-1745">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行]\*\*\*\* 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="d049d-1745">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="d049d-1747">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d049d-1747">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="d049d-1748">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1748">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="d049d-1749">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1749">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="d049d-1750">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="d049d-1750">Alternatives to Web Deploy</span></span>

<span data-ttu-id="d049d-1751">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1751">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="d049d-1752">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="d049d-1752">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="d049d-1753">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="d049d-1753">Browse the website</span></span>

<span data-ttu-id="d049d-1754">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="d049d-1754">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="d049d-1755">在下列範例中，網站繫結至 IIS 上的**連接埠** `80`，其**主機名稱**為 `www.mysite.com`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1755">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="d049d-1756">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="d049d-1756">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="d049d-1758">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="d049d-1758">Locked deployment files</span></span>

<span data-ttu-id="d049d-1759">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1759">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="d049d-1760">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1760">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="d049d-1761">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="d049d-1761">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="d049d-1762">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1762">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="d049d-1763">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="d049d-1763">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="d049d-1764">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1764">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="d049d-1765">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1765">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="d049d-1766">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1766">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="d049d-1767">使用 PowerShell 卸載*app_offline .htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="d049d-1767">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="d049d-1768">資料保護</span><span class="sxs-lookup"><span data-stu-id="d049d-1768">Data protection</span></span>

<span data-ttu-id="d049d-1769">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d049d-1769">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="d049d-1770">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1770">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="d049d-1771">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="d049d-1771">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="d049d-1772">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="d049d-1772">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="d049d-1773">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="d049d-1773">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="d049d-1774">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="d049d-1774">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="d049d-1775">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="d049d-1775">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="d049d-1776">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1776">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="d049d-1777">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="d049d-1777">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="d049d-1778">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="d049d-1778">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="d049d-1779">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="d049d-1779">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="d049d-1780">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="d049d-1780">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="d049d-1781">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1781">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="d049d-1782">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="d049d-1782">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="d049d-1783">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="d049d-1783">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="d049d-1784">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="d049d-1784">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="d049d-1785">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="d049d-1785">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="d049d-1786">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="d049d-1786">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="d049d-1787">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="d049d-1787">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="d049d-1788">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="d049d-1788">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="d049d-1789">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1789">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="d049d-1790">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="d049d-1790">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="d049d-1791">此設定位在應用程式集區 [進階設定]\*\*\*\* 下的 [處理序模型]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="d049d-1791">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="d049d-1792">將 [載入使用者設定檔]\*\*\*\* 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1792">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="d049d-1793">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="d049d-1793">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="d049d-1794">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1794">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="d049d-1795">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1795">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="d049d-1796">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1796">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="d049d-1797">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1797">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="d049d-1798">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="d049d-1798">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="d049d-1799">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d049d-1799">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="d049d-1800">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d049d-1800">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="d049d-1801">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="d049d-1801">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="d049d-1802">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1802">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="d049d-1803">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="d049d-1803">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="d049d-1804">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1804">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="d049d-1805">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="d049d-1805">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="d049d-1806">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1806">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="d049d-1807">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="d049d-1807">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="d049d-1808">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1808">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="d049d-1809">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="d049d-1809">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="d049d-1810">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1810">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="d049d-1811">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="d049d-1811">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="d049d-1812">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="d049d-1812">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="d049d-1813">如需詳細資訊，請參閱<xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="d049d-1813">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="d049d-1814">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="d049d-1814">Virtual Directories</span></span>

<span data-ttu-id="d049d-1815">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1815">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="d049d-1816">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="d049d-1816">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="d049d-1817">子應用程式</span><span class="sxs-lookup"><span data-stu-id="d049d-1817">Sub-applications</span></span>

<span data-ttu-id="d049d-1818">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="d049d-1818">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="d049d-1819">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="d049d-1819">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="d049d-1820">子應用程式不應該包括 ASP.NET Core 模組做為處理常式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1820">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="d049d-1821">如果您在子應用程式的 *web.config* 檔案中將模組新增為處理常式，則嘗試瀏覽子應用程式時，會收到參考錯誤設定檔的 *500.19 內部伺服器錯誤*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1821">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="d049d-1822">下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="d049d-1822">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="d049d-1823">在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式時，請明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="d049d-1823">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="d049d-1824">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="d049d-1824">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="d049d-1825">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="d049d-1825">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="d049d-1826">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1826">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="d049d-1827">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="d049d-1827">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="d049d-1828">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="d049d-1828">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="d049d-1829">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="d049d-1829">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="d049d-1830">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」\*\* 回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1830">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="d049d-1831">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="d049d-1831">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="d049d-1832">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1832">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="d049d-1833">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="d049d-1833">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="d049d-1834">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="d049d-1834">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="d049d-1835">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1835">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="d049d-1836">在 [新增應用程式]\*\*\*\* 對話方塊中，使用 [應用程式集區]\*\*\*\* 的[選取]\*\*\*\* 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1836">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="d049d-1837">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1837">Select **OK**.</span></span>

<span data-ttu-id="d049d-1838">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="d049d-1838">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="d049d-1839">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 。</span><span class="sxs-lookup"><span data-stu-id="d049d-1839">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="d049d-1840">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="d049d-1840">Configuration of IIS with web.config</span></span>

<span data-ttu-id="d049d-1841">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="d049d-1841">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="d049d-1842">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1842">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="d049d-1843">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1843">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="d049d-1844">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="d049d-1844">For more information, see the following topics:</span></span>

* [<span data-ttu-id="d049d-1845">的設定參考\<system.webServer></span><span class="sxs-lookup"><span data-stu-id="d049d-1845">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="d049d-1846">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數（支援 IIS 10.0 或更新版本），請參閱 IIS 參考檔中[環境變數 \<environmentVariables> ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)主題的*appcmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="d049d-1846">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="d049d-1847">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="d049d-1847">Configuration sections of web.config</span></span>

<span data-ttu-id="d049d-1848">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="d049d-1848">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="d049d-1849">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1849">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="d049d-1850">如需詳細資訊，請參閱[Configuration](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1850">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="d049d-1851">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="d049d-1851">Application Pools</span></span>

<span data-ttu-id="d049d-1852">在伺服器上裝載多個網站時，建議您在其各自的應用程式集區中執行各個應用程式，讓應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="d049d-1852">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="d049d-1853">IIS [新增網站]\*\*\*\* 對話方塊預設成此組態。</span><span class="sxs-lookup"><span data-stu-id="d049d-1853">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="d049d-1854">當提供**網站名稱**時，文字會自動轉移至 [應用程式集區]\*\*\*\* 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="d049d-1854">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="d049d-1855">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="d049d-1855">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="d049d-1856">應用程式集區Identity</span><span class="sxs-lookup"><span data-stu-id="d049d-1856">Application Pool Identity</span></span>

<span data-ttu-id="d049d-1857">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="d049d-1857">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="d049d-1858">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="d049d-1858">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="d049d-1859">在 IIS 管理主控台中，于應用程式集區的 [**高級設定**] 底下，確定 **Identity** 已設定為使用**ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="d049d-1859">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="d049d-1861">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="d049d-1861">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="d049d-1862">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="d049d-1862">Resources can be secured using this identity.</span></span> <span data-ttu-id="d049d-1863">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="d049d-1863">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="d049d-1864">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="d049d-1864">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="d049d-1865">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="d049d-1865">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="d049d-1866">以滑鼠右鍵按一下目錄並選取 [屬性]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="d049d-1866">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="d049d-1867">依序選取 [安全性]\*\*\*\* 索引標籤下的 [編輯]\*\*\*\* 按鈕和 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d049d-1867">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="d049d-1868">選取 [位置]\*\*\*\* 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="d049d-1868">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="d049d-1869">在 [輸入要選取的物件名稱]\*\*\*\* 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="d049d-1869">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="d049d-1870">選取 [檢查名稱]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d049d-1870">Select the **Check Names** button.</span></span> <span data-ttu-id="d049d-1871">針對 [DefaultAppPool]\*\*，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="d049d-1871">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="d049d-1872">選取 [檢查名稱]\*\*\*\* 按鈕時，[DefaultAppPool]\*\*\*\* 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="d049d-1872">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="d049d-1873">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="d049d-1873">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="d049d-1874">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1874">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="d049d-1876">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d049d-1876">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="d049d-1878">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-1878">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="d049d-1879">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-1879">Provide additional permissions as needed.</span></span>

<span data-ttu-id="d049d-1880">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="d049d-1880">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="d049d-1881">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="d049d-1881">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="d049d-1882">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="d049d-1882">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="d049d-1883">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="d049d-1883">HTTP/2 support</span></span>

<span data-ttu-id="d049d-1884">[HTTP/2](https://httpwg.org/specs/rfc7540.html) 支援符合下列基本需求的跨處理序部署：</span><span class="sxs-lookup"><span data-stu-id="d049d-1884">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="d049d-1885">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="d049d-1885">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="d049d-1886">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d049d-1886">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="d049d-1887">目標 Framework：不適用於跨處理序部署，因為 HTTP/2 連線完全由 IIS 處理。</span><span class="sxs-lookup"><span data-stu-id="d049d-1887">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="d049d-1888">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="d049d-1888">TLS 1.2 or later connection</span></span>

<span data-ttu-id="d049d-1889">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="d049d-1889">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="d049d-1890">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="d049d-1890">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="d049d-1891">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d049d-1891">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="d049d-1892">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1892">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="d049d-1893">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="d049d-1893">CORS preflight requests</span></span>

<span data-ttu-id="d049d-1894">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="d049d-1894">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="d049d-1895">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="d049d-1895">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="d049d-1896">若要瞭解如何在 web.config 中設定應用程式的 IIS 處理*程式來傳遞*選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="d049d-1896">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="d049d-1897">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="d049d-1897">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="d049d-1898">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="d049d-1898">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="d049d-1899">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="d049d-1899">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="d049d-1900">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="d049d-1900">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="d049d-1901">其他資源</span><span class="sxs-lookup"><span data-stu-id="d049d-1901">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="d049d-1902">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="d049d-1902">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="d049d-1903">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="d049d-1903">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="d049d-1904">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="d049d-1904">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end
