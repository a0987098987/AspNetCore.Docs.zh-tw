---
<span data-ttu-id="f6d1c-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-102">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-103">'Identity'</span></span>
- <span data-ttu-id="f6d1c-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-105">'Razor'</span></span>
- <span data-ttu-id="f6d1c-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-106">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-module"></a><span data-ttu-id="f6d1c-107">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="f6d1c-107">ASP.NET Core Module</span></span>

<span data-ttu-id="f6d1c-108">由[Tom 作者: dykstra](https://github.com/tdykstra)、 [rick Strahl](https://github.com/RickStrahl)、 [Chris Ross](https://github.com/Tratcher)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Sourabh Shirhatti](https://twitter.com/sshirhatti)和[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="f6d1c-108">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f6d1c-109">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-109">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="f6d1c-110">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-110">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="f6d1c-111">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-111">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="f6d1c-112">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-112">Supported Windows versions:</span></span>

* <span data-ttu-id="f6d1c-113">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="f6d1c-113">Windows 7 or later</span></span>
* <span data-ttu-id="f6d1c-114">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="f6d1c-114">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="f6d1c-115">同處理序裝載時，模組會使用 IIS 的同處理序伺服程式實作，稱為 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-115">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="f6d1c-116">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-116">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="f6d1c-117">此模組[無法與 HTTP.sys 搭配運作。](xref:fundamentals/servers/httpsys)</span><span class="sxs-lookup"><span data-stu-id="f6d1c-117">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="f6d1c-118">裝載模型</span><span class="sxs-lookup"><span data-stu-id="f6d1c-118">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="f6d1c-119">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="f6d1c-119">In-process hosting model</span></span>

<span data-ttu-id="f6d1c-120">ASP.NET Core 應用程式預設為同進程裝載模型。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-120">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="f6d1c-121">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-121">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="f6d1c-122">使用 IIS HTTP 伺服器 (`IISHttpServer`) 而不是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-122">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="f6d1c-123">針對同處理序，[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 來：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-123">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="f6d1c-124">註冊 `IISHttpServer`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-124">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="f6d1c-125">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-125">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="f6d1c-126">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-126">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="f6d1c-127">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-127">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="f6d1c-128">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-128">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="f6d1c-129">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-129">Use one app pool per app.</span></span>

* <span data-ttu-id="f6d1c-130">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-130">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="f6d1c-131">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-131">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="f6d1c-132">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-132">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="f6d1c-133">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-133">Client disconnects are detected.</span></span> <span data-ttu-id="f6d1c-134">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="f6d1c-135">在 ASP.NET Core 2.2.1 或更早版本中，<xref:System.IO.Directory.GetCurrentDirectory*> 會傳回 IIS 所啟動之處理序的背景工作目錄，而非應用程式的目錄 (例如 *w3wp.exe* 為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="f6d1c-136">如需設定應用程式目前所在目錄的範例程式碼，請參閱 [CurrentDirectoryHelpers 類別](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="f6d1c-137">呼叫 `SetCurrentDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="f6d1c-138">後續呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 會提供應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="f6d1c-139">裝載同處理序時，不會內部呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 來將使用者初始化。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="f6d1c-140">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="f6d1c-141">使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告時，請呼叫 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 來新增入驗證服務：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```
  
  * <span data-ttu-id="f6d1c-142">不支援[Web 套件（單一檔案）部署](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-142">[Web Package (single-file) deployments](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) aren't supported.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="f6d1c-143">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="f6d1c-143">Out-of-process hosting model</span></span>

<span data-ttu-id="f6d1c-144">若要設定跨進程裝載的應用程式，請將 `<AspNetCoreHostingModel>` `OutOfProcess` 專案檔（*.csproj*）中的屬性值設為：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-144">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="f6d1c-145">同進程裝載是使用設定的 `InProcess` ，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-145">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="f6d1c-146">的值 `<AspNetCoreHostingModel>` 不區分大小寫，因此 `inprocess` 和 `outofprocess` 都是有效的值。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-146">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="f6d1c-147">使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器而不是 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="f6d1c-148">針對跨處理序，[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 來：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="f6d1c-149">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="f6d1c-150">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-150">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="f6d1c-151">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="f6d1c-151">Hosting model changes</span></span>

<span data-ttu-id="f6d1c-152">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-152">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="f6d1c-153">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-153">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="f6d1c-154">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-154">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="f6d1c-155">程序名稱</span><span class="sxs-lookup"><span data-stu-id="f6d1c-155">Process name</span></span>

<span data-ttu-id="f6d1c-156">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-156">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="f6d1c-157">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-157">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="f6d1c-158">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-158">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="f6d1c-159">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-159">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="f6d1c-160">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-160">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="f6d1c-161">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-161">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="f6d1c-162">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-162">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="f6d1c-163">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="f6d1c-163">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="f6d1c-164">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-164">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="f6d1c-165">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="f6d1c-165">Configuration with web.config</span></span>

<span data-ttu-id="f6d1c-166">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-166">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="f6d1c-167">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-167">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet"
                  arguments=".\MyApp.dll"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="f6d1c-168">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-168">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="f6d1c-169"><xref:System.Configuration.SectionInformation.InheritInChildApplications*>屬性會設定為 `false` ，表示在專案內指定的設定 [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 不會由位於應用程式子目錄中的應用程式繼承。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-169">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="f6d1c-170">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-170">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="f6d1c-171">此路徑會將 stdout 記錄檔儲存至 [LogFiles]\*\* 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-171">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="f6d1c-172">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-172">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="f6d1c-173">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="f6d1c-173">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="f6d1c-174">屬性</span><span class="sxs-lookup"><span data-stu-id="f6d1c-174">Attribute</span></span> | <span data-ttu-id="f6d1c-175">描述</span><span class="sxs-lookup"><span data-stu-id="f6d1c-175">Description</span></span> | <span data-ttu-id="f6d1c-176">預設</span><span class="sxs-lookup"><span data-stu-id="f6d1c-176">Default</span></span> |
| ---
<span data-ttu-id="f6d1c-177">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-177">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-178">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-178">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-179">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-179">'Identity'</span></span>
- <span data-ttu-id="f6d1c-180">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-180">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-181">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-181">'Razor'</span></span>
- <span data-ttu-id="f6d1c-182">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-182">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f6d1c-183">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-183">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-184">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-184">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-185">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-185">'Identity'</span></span>
- <span data-ttu-id="f6d1c-186">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-186">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-187">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-187">'Razor'</span></span>
- <span data-ttu-id="f6d1c-188">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-188">'SignalR' uid:</span></span> 

<span data-ttu-id="f6d1c-189">----- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-189">----- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-190">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-190">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-191">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-191">'Identity'</span></span>
- <span data-ttu-id="f6d1c-192">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-192">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-193">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-193">'Razor'</span></span>
- <span data-ttu-id="f6d1c-194">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-194">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f6d1c-195">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-195">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-196">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-196">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-197">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-197">'Identity'</span></span>
- <span data-ttu-id="f6d1c-198">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-198">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-199">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-199">'Razor'</span></span>
- <span data-ttu-id="f6d1c-200">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-200">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f6d1c-201">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-201">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-202">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-202">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-203">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-203">'Identity'</span></span>
- <span data-ttu-id="f6d1c-204">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-204">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-205">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-205">'Razor'</span></span>
- <span data-ttu-id="f6d1c-206">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-206">'SignalR' uid:</span></span> 

<span data-ttu-id="f6d1c-207">------ | :-----: | | `arguments` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-207">------ | :-----: | | `arguments` | </span></span><p><span data-ttu-id="f6d1c-208">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-208">Optional string attribute.</span></span></p><p><span data-ttu-id="f6d1c-209">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-209">Arguments to the executable specified in **processPath**.</span></span></p> <span data-ttu-id="f6d1c-210">| | | `disableStartUpErrorPage` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-210">| | | `disableStartUpErrorPage` | </span></span><p><span data-ttu-id="f6d1c-211">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-211">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f6d1c-212">如果為 true，就會抑制 [502.5 - 處理序失敗]\*\*\*\* 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-212">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> <span data-ttu-id="f6d1c-213">| `false` | | `forwardWindowsAuthToken` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-213">| `false` | | `forwardWindowsAuthToken` | </span></span><p><span data-ttu-id="f6d1c-214">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-214">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f6d1c-215">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-215">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="f6d1c-216">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-216">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> <span data-ttu-id="f6d1c-217">| `true` | | `hostingModel` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-217">| `true` | | `hostingModel` | </span></span><p><span data-ttu-id="f6d1c-218">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-218">Optional string attribute.</span></span></p><p><span data-ttu-id="f6d1c-219">將主控模型指定為同進程（ `InProcess` / `inprocess` ）或跨進程（ `OutOfProcess` / `outofprocess` ）。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-219">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `InProcess`<br><span data-ttu-id="f6d1c-220">`inprocess` | | `processesPerApplication` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-220">`inprocess` | | `processesPerApplication` | </span></span><p><span data-ttu-id="f6d1c-221">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-221">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-222">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-222">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="f6d1c-223">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-223">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="f6d1c-224">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-224">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="f6d1c-225">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-225">This attribute will be removed in a future release.</span></span></p> <span data-ttu-id="f6d1c-226">|預設`1`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-226">| Default: `1`</span></span><br><span data-ttu-id="f6d1c-227">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-227">Min: `1`</span></span><br><span data-ttu-id="f6d1c-228">最大值： `100` &dagger; | |`processPath` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-228">Max: `100`&dagger; | | `processPath` | </span></span><p><span data-ttu-id="f6d1c-229">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-229">Required string attribute.</span></span></p><p><span data-ttu-id="f6d1c-230">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-230">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="f6d1c-231">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-231">Relative paths are supported.</span></span> <span data-ttu-id="f6d1c-232">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-232">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> <span data-ttu-id="f6d1c-233">| | | `rapidFailsPerMinute` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-233">| | | `rapidFailsPerMinute` | </span></span><p><span data-ttu-id="f6d1c-234">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-235">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-235">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="f6d1c-236">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-236">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="f6d1c-237">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-237">Not supported with in-process hosting.</span></span></p> <span data-ttu-id="f6d1c-238">|預設`10`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-238">| Default: `10`</span></span><br><span data-ttu-id="f6d1c-239">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-239">Min: `0`</span></span><br><span data-ttu-id="f6d1c-240">最大值： `100` | |`requestTimeout` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-240">Max: `100` | | `requestTimeout` | </span></span><p><span data-ttu-id="f6d1c-241">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-241">Optional timespan attribute.</span></span></p><p><span data-ttu-id="f6d1c-242">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-242">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="f6d1c-243">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-243">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="f6d1c-244">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-244">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="f6d1c-245">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-245">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="f6d1c-246">字串之分鐘和秒數的有效值介於 0-59。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-246">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="f6d1c-247">在分鐘或秒數的值中使用 **60** 將會導致「500 - 內部伺服器錯誤」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-247">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> <span data-ttu-id="f6d1c-248">|預設`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-248">| Default: `00:02:00`</span></span><br><span data-ttu-id="f6d1c-249">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-249">Min: `00:00:00`</span></span><br><span data-ttu-id="f6d1c-250">最大值： `360:00:00` | |`shutdownTimeLimit` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-250">Max: `360:00:00` | | `shutdownTimeLimit` | </span></span><p><span data-ttu-id="f6d1c-251">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-251">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-252">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-252">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> <span data-ttu-id="f6d1c-253">|預設`10`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-253">| Default: `10`</span></span><br><span data-ttu-id="f6d1c-254">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-254">Min: `0`</span></span><br><span data-ttu-id="f6d1c-255">最大值： `600` | |`startupTimeLimit` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-255">Max: `600` | | `startupTimeLimit` | </span></span><p><span data-ttu-id="f6d1c-256">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-256">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-257">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-257">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="f6d1c-258">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-258">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="f6d1c-259">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-259">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="f6d1c-260">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-260">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> <span data-ttu-id="f6d1c-261">|預設`120`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-261">| Default: `120`</span></span><br><span data-ttu-id="f6d1c-262">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-262">Min: `0`</span></span><br><span data-ttu-id="f6d1c-263">最大值： `3600` | |`stdoutLogEnabled` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-263">Max: `3600` | | `stdoutLogEnabled` | </span></span><p><span data-ttu-id="f6d1c-264">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-264">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f6d1c-265">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-265">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> <span data-ttu-id="f6d1c-266">| `false` | | `stdoutLogFile` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-266">| `false` | | `stdoutLogFile` | </span></span><p><span data-ttu-id="f6d1c-267">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-267">Optional string attribute.</span></span></p><p><span data-ttu-id="f6d1c-268">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-268">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="f6d1c-269">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-269">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="f6d1c-270">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-270">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="f6d1c-271">建立記錄檔後，模組會建立路徑中提供的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-271">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="f6d1c-272">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-272">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="f6d1c-273">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs]\*\* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-273">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="f6d1c-274">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="f6d1c-274">Set environment variables</span></span>

<span data-ttu-id="f6d1c-275">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-275">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="f6d1c-276">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-276">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="f6d1c-277">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-277">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="f6d1c-278">下列範例會在*web.config 中設定*兩個環境變數。將 `ASPNETCORE_ENVIRONMENT` 應用程式的環境設定為 `Development` 。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-278">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="f6d1c-279">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-279">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="f6d1c-280">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-280">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> <span data-ttu-id="f6d1c-281">直接在*web.config*中設定環境的替代方法是在 `<EnvironmentName>` [發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中包含屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-281">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="f6d1c-282">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-282">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="f6d1c-283">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-283">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="f6d1c-284">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="f6d1c-284">app_offline.htm</span></span>

<span data-ttu-id="f6d1c-285">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-285">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="f6d1c-286">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-286">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="f6d1c-287">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-287">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="f6d1c-288">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-288">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="f6d1c-289">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-289">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="f6d1c-290">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-290">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="f6d1c-291">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="f6d1c-291">Start-up error page</span></span>

<span data-ttu-id="f6d1c-292">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-292">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="f6d1c-293">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗]\*\* 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-293">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="f6d1c-294">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗]\*\* 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-294">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="f6d1c-295">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗]\*\* 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-295">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="f6d1c-296">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-296">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="f6d1c-297">如需有關設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤\<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-297">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="f6d1c-298">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="f6d1c-298">Log creation and redirection</span></span>

<span data-ttu-id="f6d1c-299">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-299">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="f6d1c-300">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-300">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="f6d1c-301">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-301">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="f6d1c-302">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-302">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="f6d1c-303">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-303">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="f6d1c-304">僅建議使用 stdout 記錄檔，以便在 IIS 上裝載時或使用[iis Visual Studio 的開發時間支援](xref:host-and-deploy/iis/development-time-iis-support)時，針對應用程式啟動問題進行疑難排解，而不是在本機進行偵錯工具並使用 IIS Express 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-304">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="f6d1c-305">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-305">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="f6d1c-306">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-306">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f6d1c-307">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-307">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="f6d1c-308">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-308">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="f6d1c-309">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 (*.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-309">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="f6d1c-310">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-310">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="f6d1c-311">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-311">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="f6d1c-312">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-312">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="f6d1c-313">下列範例 `aspNetCore` 元素會在相對路徑上設定 stdout 記錄 `.\log\` 。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-313">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="f6d1c-314">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-314">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="f6d1c-315">發行 Azure App Service 部署的應用程式時，Web SDK 會將 `stdoutLogFile` 值設定為 `\\?\%home%\LogFiles\stdout` 。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-315">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="f6d1c-316">`%home`環境變數是針對 Azure App Service 所主控的應用程式預先定義的。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-316">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="f6d1c-317">若要建立記錄篩選規則，請參閱 ASP.NET Core 記錄檔的[設定和](xref:fundamentals/logging/index#log-filtering)[記錄篩選](xref:fundamentals/logging/index#log-filtering)區段。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-317">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="f6d1c-318">如需路徑格式的詳細資訊，請參閱[Windows 系統上的檔案路徑格式](/dotnet/standard/io/file-path-formats)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-318">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="f6d1c-319">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="f6d1c-319">Enhanced diagnostic logs</span></span>

<span data-ttu-id="f6d1c-320">ASP.NET Core 模組是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-320">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="f6d1c-321">將 `<handlerSettings>` 元素加入至 web.config `<aspNetCore>` 中的*web.config*專案。將設定 `debugLevel` 為會 `TRACE` 公開更高的診斷資訊精確度：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-321">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="f6d1c-322">建立記錄檔後，模組會建立路徑 (在上述範例中為 *logs*) 中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-322">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="f6d1c-323">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-323">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="f6d1c-324">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-324">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="f6d1c-325">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-325">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="f6d1c-326">ERROR</span><span class="sxs-lookup"><span data-stu-id="f6d1c-326">ERROR</span></span>
* <span data-ttu-id="f6d1c-327">WARNING</span><span class="sxs-lookup"><span data-stu-id="f6d1c-327">WARNING</span></span>
* <span data-ttu-id="f6d1c-328">INFO</span><span class="sxs-lookup"><span data-stu-id="f6d1c-328">INFO</span></span>
* <span data-ttu-id="f6d1c-329">TRACE</span><span class="sxs-lookup"><span data-stu-id="f6d1c-329">TRACE</span></span>

<span data-ttu-id="f6d1c-330">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-330">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="f6d1c-331">主控台</span><span class="sxs-lookup"><span data-stu-id="f6d1c-331">CONSOLE</span></span>
* <span data-ttu-id="f6d1c-332">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="f6d1c-332">EVENTLOG</span></span>
* <span data-ttu-id="f6d1c-333">FILE</span><span class="sxs-lookup"><span data-stu-id="f6d1c-333">FILE</span></span>

<span data-ttu-id="f6d1c-334">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-334">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="f6d1c-335">`ASPNETCORE_MODULE_DEBUG_FILE`： Debug 記錄檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-335">`ASPNETCORE_MODULE_DEBUG_FILE`: Path to the debug log file.</span></span> <span data-ttu-id="f6d1c-336">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="f6d1c-336">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="f6d1c-337">`ASPNETCORE_MODULE_DEBUG`： Debug level 設定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-337">`ASPNETCORE_MODULE_DEBUG`: Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="f6d1c-338">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-338">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="f6d1c-339">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-339">The size of the log isn't limited.</span></span> <span data-ttu-id="f6d1c-340">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-340">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="f6d1c-341">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-341">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="f6d1c-342">修改堆疊大小</span><span class="sxs-lookup"><span data-stu-id="f6d1c-342">Modify the stack size</span></span>

<span data-ttu-id="f6d1c-343">*僅適用于使用同進程裝載模型時。*</span><span class="sxs-lookup"><span data-stu-id="f6d1c-343">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="f6d1c-344">使用 web.config 中的 `stackSize` 設定（以位元組為單位） *web.config*來設定受控堆疊大小。預設大小為 `1048576` 位元組（1 MB）。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-344">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="f6d1c-345">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="f6d1c-345">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="f6d1c-346">僅適用於跨處理序裝載。\*\*</span><span class="sxs-lookup"><span data-stu-id="f6d1c-346">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="f6d1c-347">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-347">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="f6d1c-348">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-348">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="f6d1c-349">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-349">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="f6d1c-350">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-350">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="f6d1c-351">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-351">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="f6d1c-352">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-352">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="f6d1c-353">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-353">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="f6d1c-354">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-354">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="f6d1c-355">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-355">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="f6d1c-356">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="f6d1c-356">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="f6d1c-357">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-357">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="f6d1c-358">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-358">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="f6d1c-359">在與 IIS 安裝相同的電腦上使用 IIS 共用設定時，請執行 ASP.NET Core 裝載套件組合安裝程式並將 `OPT_NO_SHARED_CONFIG_CHECK` 參數設為 `1`：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-359">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="f6d1c-360">若共用設定的路徑位於與 IIS 安裝不同的電腦上，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-360">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="f6d1c-361">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-361">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="f6d1c-362">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-362">Run the installer.</span></span>
1. <span data-ttu-id="f6d1c-363">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-363">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="f6d1c-364">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-364">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="f6d1c-365">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="f6d1c-365">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="f6d1c-366">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-366">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="f6d1c-367">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-367">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="f6d1c-368">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-368">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="f6d1c-369">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-369">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="f6d1c-370">選取 [**詳細資料**] 索引標籤。檔案**版本**和**產品版本**代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-370">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="f6d1c-371">模組的裝載套件組合安裝程式記錄檔位於*C： \\ Users \\ % UserName% \\ AppData \\ Local \\ Temp*。此檔案的名稱為*dd_DotNetCoreWinSvrHosting__ \<timestamp> _000_AspNetCoreModule_x64 .log*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-371">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="f6d1c-372">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="f6d1c-372">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="f6d1c-373">模組</span><span class="sxs-lookup"><span data-stu-id="f6d1c-373">Module</span></span>

<span data-ttu-id="f6d1c-374">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-374">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="f6d1c-375">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-375">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="f6d1c-376">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-376">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="f6d1c-377">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-377">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="f6d1c-378">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-378">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="f6d1c-379">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-379">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="f6d1c-380">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-380">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="f6d1c-381">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-381">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="f6d1c-382">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-382">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="f6d1c-383">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-383">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="f6d1c-384">結構描述</span><span class="sxs-lookup"><span data-stu-id="f6d1c-384">Schema</span></span>

<span data-ttu-id="f6d1c-385">**IIS**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-385">**IIS**</span></span>

* <span data-ttu-id="f6d1c-386">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="f6d1c-386">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="f6d1c-387">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="f6d1c-387">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="f6d1c-388">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-388">**IIS Express**</span></span>

* <span data-ttu-id="f6d1c-389">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="f6d1c-389">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="f6d1c-390">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="f6d1c-390">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="f6d1c-391">組態</span><span class="sxs-lookup"><span data-stu-id="f6d1c-391">Configuration</span></span>

<span data-ttu-id="f6d1c-392">**IIS**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-392">**IIS**</span></span>

* <span data-ttu-id="f6d1c-393">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="f6d1c-393">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="f6d1c-394">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-394">**IIS Express**</span></span>

* <span data-ttu-id="f6d1c-395">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="f6d1c-395">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="f6d1c-396">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="f6d1c-396">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="f6d1c-397">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-397">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="f6d1c-398">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-398">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="f6d1c-399">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-399">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="f6d1c-400">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-400">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="f6d1c-401">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-401">Supported Windows versions:</span></span>

* <span data-ttu-id="f6d1c-402">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="f6d1c-402">Windows 7 or later</span></span>
* <span data-ttu-id="f6d1c-403">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="f6d1c-403">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="f6d1c-404">同處理序裝載時，模組會使用 IIS 的同處理序伺服程式實作，稱為 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-404">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="f6d1c-405">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-405">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="f6d1c-406">此模組[無法與 HTTP.sys 搭配運作。](xref:fundamentals/servers/httpsys)</span><span class="sxs-lookup"><span data-stu-id="f6d1c-406">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="f6d1c-407">裝載模型</span><span class="sxs-lookup"><span data-stu-id="f6d1c-407">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="f6d1c-408">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="f6d1c-408">In-process hosting model</span></span>

<span data-ttu-id="f6d1c-409">若要設定同處理序裝載的應用程式，請將 `<AspNetCoreHostingModel>` 屬性新增至應用程式的專案檔，其值為 `InProcess` (跨處理序裝載是使用 `OutOfProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-409">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="f6d1c-410">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-410">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="f6d1c-411">的值 `<AspNetCoreHostingModel>` 不區分大小寫，因此 `inprocess` 和 `outofprocess` 都是有效的值。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-411">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="f6d1c-412">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-412">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="f6d1c-413">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-413">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="f6d1c-414">使用 IIS HTTP 伺服器 (`IISHttpServer`) 而不是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-414">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="f6d1c-415">針對同處理序，[CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 來：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-415">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="f6d1c-416">註冊 `IISHttpServer`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-416">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="f6d1c-417">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-417">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="f6d1c-418">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-418">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="f6d1c-419">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-419">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="f6d1c-420">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-420">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="f6d1c-421">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-421">Use one app pool per app.</span></span>

* <span data-ttu-id="f6d1c-422">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-422">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="f6d1c-423">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-423">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="f6d1c-424">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-424">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="f6d1c-425">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-425">Client disconnects are detected.</span></span> <span data-ttu-id="f6d1c-426">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-426">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="f6d1c-427">在 ASP.NET Core 2.2.1 或更早版本中，<xref:System.IO.Directory.GetCurrentDirectory*> 會傳回 IIS 所啟動之處理序的背景工作目錄，而非應用程式的目錄 (例如 *w3wp.exe* 為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-427">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="f6d1c-428">如需設定應用程式目前所在目錄的範例程式碼，請參閱 [CurrentDirectoryHelpers 類別](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-428">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="f6d1c-429">呼叫 `SetCurrentDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-429">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="f6d1c-430">後續呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 會提供應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-430">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="f6d1c-431">裝載同處理序時，不會內部呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 來將使用者初始化。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-431">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="f6d1c-432">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-432">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="f6d1c-433">使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告時，請呼叫 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 來新增入驗證服務：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-433">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="f6d1c-434">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="f6d1c-434">Out-of-process hosting model</span></span>

<span data-ttu-id="f6d1c-435">若要設定跨處理序裝載的應用程式，請在專案檔中使用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-435">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="f6d1c-436">請勿指定 `<AspNetCoreHostingModel>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-436">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="f6d1c-437">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-437">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="f6d1c-438">將 `<AspNetCoreHostingModel>` 屬性的值設定為 `OutOfProcess` (同處理序裝載是使用 `InProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-438">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="f6d1c-439">此值不區分大小寫，因此 `inprocess` 和 `outofprocess` 都是有效的值。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-439">The value is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="f6d1c-440">使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器而不是 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-440">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="f6d1c-441">針對跨處理序，[CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 來：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-441">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="f6d1c-442">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-442">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="f6d1c-443">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-443">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="f6d1c-444">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="f6d1c-444">Hosting model changes</span></span>

<span data-ttu-id="f6d1c-445">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-445">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="f6d1c-446">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-446">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="f6d1c-447">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-447">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="f6d1c-448">程序名稱</span><span class="sxs-lookup"><span data-stu-id="f6d1c-448">Process name</span></span>

<span data-ttu-id="f6d1c-449">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-449">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="f6d1c-450">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-450">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="f6d1c-451">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-451">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="f6d1c-452">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-452">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="f6d1c-453">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-453">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="f6d1c-454">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-454">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="f6d1c-455">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-455">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="f6d1c-456">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="f6d1c-456">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="f6d1c-457">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-457">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="f6d1c-458">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="f6d1c-458">Configuration with web.config</span></span>

<span data-ttu-id="f6d1c-459">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-459">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="f6d1c-460">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-460">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet"
                  arguments=".\MyApp.dll"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="f6d1c-461">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-461">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="f6d1c-462"><xref:System.Configuration.SectionInformation.InheritInChildApplications*>屬性會設定為 `false` ，表示在專案內指定的設定 [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 不會由位於應用程式子目錄中的應用程式繼承。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-462">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="f6d1c-463">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-463">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="f6d1c-464">此路徑會將 stdout 記錄檔儲存至 [LogFiles]\*\* 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-464">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="f6d1c-465">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-465">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="f6d1c-466">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="f6d1c-466">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="f6d1c-467">屬性</span><span class="sxs-lookup"><span data-stu-id="f6d1c-467">Attribute</span></span> | <span data-ttu-id="f6d1c-468">描述</span><span class="sxs-lookup"><span data-stu-id="f6d1c-468">Description</span></span> | <span data-ttu-id="f6d1c-469">預設</span><span class="sxs-lookup"><span data-stu-id="f6d1c-469">Default</span></span> |
| ---
<span data-ttu-id="f6d1c-470">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-470">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-471">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-471">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-472">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-472">'Identity'</span></span>
- <span data-ttu-id="f6d1c-473">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-473">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-474">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-474">'Razor'</span></span>
- <span data-ttu-id="f6d1c-475">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-475">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f6d1c-476">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-476">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-477">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-477">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-478">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-478">'Identity'</span></span>
- <span data-ttu-id="f6d1c-479">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-479">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-480">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-480">'Razor'</span></span>
- <span data-ttu-id="f6d1c-481">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-481">'SignalR' uid:</span></span> 

<span data-ttu-id="f6d1c-482">----- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-482">----- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-483">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-483">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-484">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-484">'Identity'</span></span>
- <span data-ttu-id="f6d1c-485">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-485">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-486">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-486">'Razor'</span></span>
- <span data-ttu-id="f6d1c-487">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-487">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f6d1c-488">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-488">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-489">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-489">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-490">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-490">'Identity'</span></span>
- <span data-ttu-id="f6d1c-491">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-491">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-492">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-492">'Razor'</span></span>
- <span data-ttu-id="f6d1c-493">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-493">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f6d1c-494">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-494">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-495">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-495">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-496">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-496">'Identity'</span></span>
- <span data-ttu-id="f6d1c-497">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-497">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-498">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-498">'Razor'</span></span>
- <span data-ttu-id="f6d1c-499">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-499">'SignalR' uid:</span></span> 

<span data-ttu-id="f6d1c-500">------ | :-----: | | `arguments` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-500">------ | :-----: | | `arguments` | </span></span><p><span data-ttu-id="f6d1c-501">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-501">Optional string attribute.</span></span></p><p><span data-ttu-id="f6d1c-502">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-502">Arguments to the executable specified in **processPath**.</span></span></p> <span data-ttu-id="f6d1c-503">| | | `disableStartUpErrorPage` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-503">| | | `disableStartUpErrorPage` | </span></span><p><span data-ttu-id="f6d1c-504">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-504">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f6d1c-505">如果為 true，就會抑制 [502.5 - 處理序失敗]\*\*\*\* 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-505">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> <span data-ttu-id="f6d1c-506">| `false` | | `forwardWindowsAuthToken` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-506">| `false` | | `forwardWindowsAuthToken` | </span></span><p><span data-ttu-id="f6d1c-507">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-507">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f6d1c-508">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-508">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="f6d1c-509">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-509">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> <span data-ttu-id="f6d1c-510">| `true` | | `hostingModel` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-510">| `true` | | `hostingModel` | </span></span><p><span data-ttu-id="f6d1c-511">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-511">Optional string attribute.</span></span></p><p><span data-ttu-id="f6d1c-512">將主控模型指定為同進程（ `InProcess` / `inprocess` ）或跨進程（ `OutOfProcess` / `outofprocess` ）。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-512">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `OutOfProcess`<br><span data-ttu-id="f6d1c-513">`outofprocess` | | `processesPerApplication` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-513">`outofprocess` | | `processesPerApplication` | </span></span><p><span data-ttu-id="f6d1c-514">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-514">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-515">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-515">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="f6d1c-516">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-516">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="f6d1c-517">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-517">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="f6d1c-518">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-518">This attribute will be removed in a future release.</span></span></p> <span data-ttu-id="f6d1c-519">|預設`1`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-519">| Default: `1`</span></span><br><span data-ttu-id="f6d1c-520">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-520">Min: `1`</span></span><br><span data-ttu-id="f6d1c-521">最大值： `100` &dagger; | |`processPath` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-521">Max: `100`&dagger; | | `processPath` | </span></span><p><span data-ttu-id="f6d1c-522">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-522">Required string attribute.</span></span></p><p><span data-ttu-id="f6d1c-523">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-523">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="f6d1c-524">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-524">Relative paths are supported.</span></span> <span data-ttu-id="f6d1c-525">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-525">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> <span data-ttu-id="f6d1c-526">| | | `rapidFailsPerMinute` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-526">| | | `rapidFailsPerMinute` | </span></span><p><span data-ttu-id="f6d1c-527">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-527">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-528">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-528">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="f6d1c-529">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-529">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="f6d1c-530">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-530">Not supported with in-process hosting.</span></span></p> <span data-ttu-id="f6d1c-531">|預設`10`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-531">| Default: `10`</span></span><br><span data-ttu-id="f6d1c-532">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-532">Min: `0`</span></span><br><span data-ttu-id="f6d1c-533">最大值： `100` | |`requestTimeout` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-533">Max: `100` | | `requestTimeout` | </span></span><p><span data-ttu-id="f6d1c-534">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-534">Optional timespan attribute.</span></span></p><p><span data-ttu-id="f6d1c-535">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-535">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="f6d1c-536">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-536">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="f6d1c-537">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-537">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="f6d1c-538">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-538">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="f6d1c-539">字串之分鐘和秒數的有效值介於 0-59。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-539">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="f6d1c-540">在分鐘或秒數的值中使用 **60** 將會導致「500 - 內部伺服器錯誤」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-540">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> <span data-ttu-id="f6d1c-541">|預設`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-541">| Default: `00:02:00`</span></span><br><span data-ttu-id="f6d1c-542">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-542">Min: `00:00:00`</span></span><br><span data-ttu-id="f6d1c-543">最大值： `360:00:00` | |`shutdownTimeLimit` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-543">Max: `360:00:00` | | `shutdownTimeLimit` | </span></span><p><span data-ttu-id="f6d1c-544">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-544">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-545">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-545">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> <span data-ttu-id="f6d1c-546">|預設`10`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-546">| Default: `10`</span></span><br><span data-ttu-id="f6d1c-547">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-547">Min: `0`</span></span><br><span data-ttu-id="f6d1c-548">最大值： `600` | |`startupTimeLimit` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-548">Max: `600` | | `startupTimeLimit` | </span></span><p><span data-ttu-id="f6d1c-549">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-549">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-550">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-550">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="f6d1c-551">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-551">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="f6d1c-552">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-552">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="f6d1c-553">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-553">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> <span data-ttu-id="f6d1c-554">|預設`120`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-554">| Default: `120`</span></span><br><span data-ttu-id="f6d1c-555">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-555">Min: `0`</span></span><br><span data-ttu-id="f6d1c-556">最大值： `3600` | |`stdoutLogEnabled` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-556">Max: `3600` | | `stdoutLogEnabled` | </span></span><p><span data-ttu-id="f6d1c-557">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-557">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f6d1c-558">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-558">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> <span data-ttu-id="f6d1c-559">| `false` | | `stdoutLogFile` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-559">| `false` | | `stdoutLogFile` | </span></span><p><span data-ttu-id="f6d1c-560">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-560">Optional string attribute.</span></span></p><p><span data-ttu-id="f6d1c-561">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-561">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="f6d1c-562">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-562">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="f6d1c-563">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-563">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="f6d1c-564">建立記錄檔後，模組會建立路徑中提供的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-564">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="f6d1c-565">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-565">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="f6d1c-566">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs]\*\* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-566">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="f6d1c-567">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="f6d1c-567">Setting environment variables</span></span>

<span data-ttu-id="f6d1c-568">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-568">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="f6d1c-569">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-569">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="f6d1c-570">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-570">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="f6d1c-571">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-571">The following example sets two environment variables.</span></span> <span data-ttu-id="f6d1c-572">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-572">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="f6d1c-573">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-573">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="f6d1c-574">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-574">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> <span data-ttu-id="f6d1c-575">直接在*web.config*中設定環境的替代方法是在 `<EnvironmentName>` [發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中包含屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-575">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="f6d1c-576">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-576">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="f6d1c-577">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-577">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="f6d1c-578">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="f6d1c-578">app_offline.htm</span></span>

<span data-ttu-id="f6d1c-579">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-579">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="f6d1c-580">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-580">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="f6d1c-581">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-581">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="f6d1c-582">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-582">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="f6d1c-583">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-583">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="f6d1c-584">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-584">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="f6d1c-585">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="f6d1c-585">Start-up error page</span></span>

<span data-ttu-id="f6d1c-586">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-586">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="f6d1c-587">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗]\*\* 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-587">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="f6d1c-588">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗]\*\* 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-588">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="f6d1c-589">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗]\*\* 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-589">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="f6d1c-590">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-590">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="f6d1c-591">如需有關設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤\<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-591">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="f6d1c-592">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="f6d1c-592">Log creation and redirection</span></span>

<span data-ttu-id="f6d1c-593">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-593">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="f6d1c-594">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-594">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="f6d1c-595">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-595">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="f6d1c-596">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-596">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="f6d1c-597">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-597">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="f6d1c-598">僅建議使用 stdout 記錄檔，以便在 IIS 上裝載時或使用[iis Visual Studio 的開發時間支援](xref:host-and-deploy/iis/development-time-iis-support)時，針對應用程式啟動問題進行疑難排解，而不是在本機進行偵錯工具並使用 IIS Express 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-598">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="f6d1c-599">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-599">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="f6d1c-600">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-600">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f6d1c-601">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-601">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="f6d1c-602">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-602">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="f6d1c-603">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 (*.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-603">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="f6d1c-604">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-604">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="f6d1c-605">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-605">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="f6d1c-606">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-606">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="f6d1c-607">下列範例 `aspNetCore` 元素會在相對路徑上設定 stdout 記錄 `.\log\` 。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-607">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="f6d1c-608">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-608">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="f6d1c-609">發行 Azure App Service 部署的應用程式時，Web SDK 會將 `stdoutLogFile` 值設定為 `\\?\%home%\LogFiles\stdout` 。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-609">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="f6d1c-610">`%home`環境變數是針對 Azure App Service 所主控的應用程式預先定義的。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-610">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="f6d1c-611">如需路徑格式的詳細資訊，請參閱[Windows 系統上的檔案路徑格式](/dotnet/standard/io/file-path-formats)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-611">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="f6d1c-612">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="f6d1c-612">Enhanced diagnostic logs</span></span>

<span data-ttu-id="f6d1c-613">ASP.NET Core 模組是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-613">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="f6d1c-614">將 `<handlerSettings>` 元素加入至 web.config `<aspNetCore>` 中的*web.config*專案。將設定 `debugLevel` 為會 `TRACE` 公開更高的診斷資訊精確度：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-614">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="f6d1c-615">模組不會在提供給 `<handlerSetting>` 值的路徑 (在上述範例中為 *logs*) 中自動建立資料夾，而且這些資料夾應該已存在於部署中。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-615">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="f6d1c-616">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-616">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="f6d1c-617">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-617">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="f6d1c-618">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-618">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="f6d1c-619">ERROR</span><span class="sxs-lookup"><span data-stu-id="f6d1c-619">ERROR</span></span>
* <span data-ttu-id="f6d1c-620">WARNING</span><span class="sxs-lookup"><span data-stu-id="f6d1c-620">WARNING</span></span>
* <span data-ttu-id="f6d1c-621">INFO</span><span class="sxs-lookup"><span data-stu-id="f6d1c-621">INFO</span></span>
* <span data-ttu-id="f6d1c-622">TRACE</span><span class="sxs-lookup"><span data-stu-id="f6d1c-622">TRACE</span></span>

<span data-ttu-id="f6d1c-623">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-623">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="f6d1c-624">主控台</span><span class="sxs-lookup"><span data-stu-id="f6d1c-624">CONSOLE</span></span>
* <span data-ttu-id="f6d1c-625">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="f6d1c-625">EVENTLOG</span></span>
* <span data-ttu-id="f6d1c-626">FILE</span><span class="sxs-lookup"><span data-stu-id="f6d1c-626">FILE</span></span>

<span data-ttu-id="f6d1c-627">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-627">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="f6d1c-628">`ASPNETCORE_MODULE_DEBUG_FILE`： Debug 記錄檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-628">`ASPNETCORE_MODULE_DEBUG_FILE`: Path to the debug log file.</span></span> <span data-ttu-id="f6d1c-629">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="f6d1c-629">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="f6d1c-630">`ASPNETCORE_MODULE_DEBUG`： Debug level 設定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-630">`ASPNETCORE_MODULE_DEBUG`: Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="f6d1c-631">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-631">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="f6d1c-632">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-632">The size of the log isn't limited.</span></span> <span data-ttu-id="f6d1c-633">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-633">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="f6d1c-634">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-634">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="f6d1c-635">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="f6d1c-635">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="f6d1c-636">僅適用於跨處理序裝載。\*\*</span><span class="sxs-lookup"><span data-stu-id="f6d1c-636">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="f6d1c-637">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-637">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="f6d1c-638">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-638">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="f6d1c-639">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-639">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="f6d1c-640">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-640">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="f6d1c-641">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-641">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="f6d1c-642">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-642">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="f6d1c-643">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-643">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="f6d1c-644">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-644">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="f6d1c-645">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-645">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="f6d1c-646">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="f6d1c-646">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="f6d1c-647">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-647">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="f6d1c-648">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-648">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="f6d1c-649">在與 IIS 安裝相同的電腦上使用 IIS 共用設定時，請執行 ASP.NET Core 裝載套件組合安裝程式並將 `OPT_NO_SHARED_CONFIG_CHECK` 參數設為 `1`：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-649">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="f6d1c-650">若共用設定的路徑位於與 IIS 安裝不同的電腦上，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-650">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="f6d1c-651">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-651">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="f6d1c-652">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-652">Run the installer.</span></span>
1. <span data-ttu-id="f6d1c-653">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-653">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="f6d1c-654">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-654">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="f6d1c-655">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="f6d1c-655">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="f6d1c-656">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-656">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="f6d1c-657">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-657">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="f6d1c-658">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-658">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="f6d1c-659">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-659">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="f6d1c-660">選取 [**詳細資料**] 索引標籤。檔案**版本**和**產品版本**代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-660">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="f6d1c-661">模組的裝載套件組合安裝程式記錄檔位於*C： \\ Users \\ % UserName% \\ AppData \\ Local \\ Temp*。此檔案的名稱為*dd_DotNetCoreWinSvrHosting__ \<timestamp> _000_AspNetCoreModule_x64 .log*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-661">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="f6d1c-662">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="f6d1c-662">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="f6d1c-663">模組</span><span class="sxs-lookup"><span data-stu-id="f6d1c-663">Module</span></span>

<span data-ttu-id="f6d1c-664">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-664">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="f6d1c-665">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-665">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="f6d1c-666">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-666">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="f6d1c-667">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-667">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="f6d1c-668">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-668">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="f6d1c-669">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-669">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="f6d1c-670">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-670">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="f6d1c-671">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-671">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="f6d1c-672">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-672">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="f6d1c-673">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-673">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="f6d1c-674">結構描述</span><span class="sxs-lookup"><span data-stu-id="f6d1c-674">Schema</span></span>

<span data-ttu-id="f6d1c-675">**IIS**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-675">**IIS**</span></span>

* <span data-ttu-id="f6d1c-676">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="f6d1c-676">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="f6d1c-677">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="f6d1c-677">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="f6d1c-678">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-678">**IIS Express**</span></span>

* <span data-ttu-id="f6d1c-679">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="f6d1c-679">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="f6d1c-680">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="f6d1c-680">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="f6d1c-681">組態</span><span class="sxs-lookup"><span data-stu-id="f6d1c-681">Configuration</span></span>

<span data-ttu-id="f6d1c-682">**IIS**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-682">**IIS**</span></span>

* <span data-ttu-id="f6d1c-683">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="f6d1c-683">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="f6d1c-684">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-684">**IIS Express**</span></span>

* <span data-ttu-id="f6d1c-685">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="f6d1c-685">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="f6d1c-686">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="f6d1c-686">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="f6d1c-687">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-687">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="f6d1c-688">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線，將 Web 要求重新轉送到後端的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-688">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="f6d1c-689">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-689">Supported Windows versions:</span></span>

* <span data-ttu-id="f6d1c-690">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="f6d1c-690">Windows 7 or later</span></span>
* <span data-ttu-id="f6d1c-691">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="f6d1c-691">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="f6d1c-692">該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-692">The module only works with Kestrel.</span></span> <span data-ttu-id="f6d1c-693">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) 不相容。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-693">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="f6d1c-694">因為處理序中執行的 ASP.NET Core 應用程式會與 IIS 背景工作處理序分開，所以此模組也會執行處理序管理。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-694">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="f6d1c-695">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-695">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="f6d1c-696">此行為基本上與在 IIS 中執行同處理序，並由 [Windows 處理器啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的 ASP.NET 4.x 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-696">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="f6d1c-697">下圖說明 IIS、ASP.NET Core 模組和應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-697">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core 模組](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="f6d1c-699">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-699">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="f6d1c-700">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-700">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="f6d1c-701">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-701">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="f6d1c-702">此模組會在啟動時透過環境變數指定埠，而[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)會設定伺服器來接聽 `http://localhost:{port}` 。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-702">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="f6d1c-703">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-703">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="f6d1c-704">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-704">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="f6d1c-705">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-705">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="f6d1c-706">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-706">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="f6d1c-707">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-707">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="f6d1c-708">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-708">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="f6d1c-709">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-709">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="f6d1c-710">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-710">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="f6d1c-711">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-711">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="f6d1c-712">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-712">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="f6d1c-713">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-713">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="f6d1c-714">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-714">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="f6d1c-715">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="f6d1c-715">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="f6d1c-716">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-716">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="f6d1c-717">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="f6d1c-717">Configuration with web.config</span></span>

<span data-ttu-id="f6d1c-718">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-718">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="f6d1c-719">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-719">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet"
                arguments=".\MyApp.dll"
                stdoutLogEnabled="false"
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="f6d1c-720">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-720">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe"
                stdoutLogEnabled="false"
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="f6d1c-721">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-721">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="f6d1c-722">此路徑會將 stdout 記錄檔儲存至 [LogFiles]\*\* 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-722">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="f6d1c-723">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-723">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="f6d1c-724">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="f6d1c-724">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="f6d1c-725">屬性</span><span class="sxs-lookup"><span data-stu-id="f6d1c-725">Attribute</span></span> | <span data-ttu-id="f6d1c-726">描述</span><span class="sxs-lookup"><span data-stu-id="f6d1c-726">Description</span></span> | <span data-ttu-id="f6d1c-727">預設</span><span class="sxs-lookup"><span data-stu-id="f6d1c-727">Default</span></span> |
| ---
<span data-ttu-id="f6d1c-728">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-728">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-729">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-729">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-730">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-730">'Identity'</span></span>
- <span data-ttu-id="f6d1c-731">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-731">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-732">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-732">'Razor'</span></span>
- <span data-ttu-id="f6d1c-733">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-733">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f6d1c-734">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-734">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-735">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-735">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-736">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-736">'Identity'</span></span>
- <span data-ttu-id="f6d1c-737">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-737">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-738">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-738">'Razor'</span></span>
- <span data-ttu-id="f6d1c-739">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-739">'SignalR' uid:</span></span> 

<span data-ttu-id="f6d1c-740">----- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-740">----- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-741">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-741">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-742">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-742">'Identity'</span></span>
- <span data-ttu-id="f6d1c-743">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-743">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-744">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-744">'Razor'</span></span>
- <span data-ttu-id="f6d1c-745">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-745">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f6d1c-746">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-746">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-747">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-747">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-748">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-748">'Identity'</span></span>
- <span data-ttu-id="f6d1c-749">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-749">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-750">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-750">'Razor'</span></span>
- <span data-ttu-id="f6d1c-751">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-751">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f6d1c-752">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-752">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6d1c-753">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-753">'Blazor'</span></span>
- <span data-ttu-id="f6d1c-754">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-754">'Identity'</span></span>
- <span data-ttu-id="f6d1c-755">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-755">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6d1c-756">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6d1c-756">'Razor'</span></span>
- <span data-ttu-id="f6d1c-757">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-757">'SignalR' uid:</span></span> 

<span data-ttu-id="f6d1c-758">------ | :-----: | | `arguments` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-758">------ | :-----: | | `arguments` | </span></span><p><span data-ttu-id="f6d1c-759">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-759">Optional string attribute.</span></span></p><p><span data-ttu-id="f6d1c-760">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-760">Arguments to the executable specified in **processPath**.</span></span></p><span data-ttu-id="f6d1c-761">| | | `disableStartUpErrorPage` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-761">| | | `disableStartUpErrorPage` | </span></span><p><span data-ttu-id="f6d1c-762">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-762">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f6d1c-763">如果為 true，就會抑制 [502.5 - 處理序失敗]\*\*\*\* 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-763">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> <span data-ttu-id="f6d1c-764">| `false` | | `forwardWindowsAuthToken` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-764">| `false` | | `forwardWindowsAuthToken` | </span></span><p><span data-ttu-id="f6d1c-765">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-765">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f6d1c-766">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-766">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="f6d1c-767">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-767">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> <span data-ttu-id="f6d1c-768">| `true` | | `processesPerApplication` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-768">| `true` | | `processesPerApplication` | </span></span><p><span data-ttu-id="f6d1c-769">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-769">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-770">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-770">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="f6d1c-771">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-771">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="f6d1c-772">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-772">This attribute will be removed in a future release.</span></span></p> <span data-ttu-id="f6d1c-773">|預設`1`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-773">| Default: `1`</span></span><br><span data-ttu-id="f6d1c-774">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-774">Min: `1`</span></span><br><span data-ttu-id="f6d1c-775">最大值： `100` | |`processPath` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-775">Max: `100` | | `processPath` | </span></span><p><span data-ttu-id="f6d1c-776">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-776">Required string attribute.</span></span></p><p><span data-ttu-id="f6d1c-777">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-777">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="f6d1c-778">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-778">Relative paths are supported.</span></span> <span data-ttu-id="f6d1c-779">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-779">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> <span data-ttu-id="f6d1c-780">| | | `rapidFailsPerMinute` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-780">| | | `rapidFailsPerMinute` | </span></span><p><span data-ttu-id="f6d1c-781">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-781">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-782">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-782">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="f6d1c-783">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-783">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> <span data-ttu-id="f6d1c-784">|預設`10`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-784">| Default: `10`</span></span><br><span data-ttu-id="f6d1c-785">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-785">Min: `0`</span></span><br><span data-ttu-id="f6d1c-786">最大值： `100` | |`requestTimeout` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-786">Max: `100` | | `requestTimeout` | </span></span><p><span data-ttu-id="f6d1c-787">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-787">Optional timespan attribute.</span></span></p><p><span data-ttu-id="f6d1c-788">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-788">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="f6d1c-789">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-789">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> <span data-ttu-id="f6d1c-790">|預設`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-790">| Default: `00:02:00`</span></span><br><span data-ttu-id="f6d1c-791">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-791">Min: `00:00:00`</span></span><br><span data-ttu-id="f6d1c-792">最大值： `360:00:00` | |`shutdownTimeLimit` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-792">Max: `360:00:00` | | `shutdownTimeLimit` | </span></span><p><span data-ttu-id="f6d1c-793">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-793">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-794">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-794">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> <span data-ttu-id="f6d1c-795">|預設`10`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-795">| Default: `10`</span></span><br><span data-ttu-id="f6d1c-796">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-796">Min: `0`</span></span><br><span data-ttu-id="f6d1c-797">最大值： `600` | |`startupTimeLimit` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-797">Max: `600` | | `startupTimeLimit` | </span></span><p><span data-ttu-id="f6d1c-798">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-798">Optional integer attribute.</span></span></p><p><span data-ttu-id="f6d1c-799">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-799">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="f6d1c-800">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-800">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="f6d1c-801">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-801">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="f6d1c-802">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-802">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> <span data-ttu-id="f6d1c-803">|預設`120`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-803">| Default: `120`</span></span><br><span data-ttu-id="f6d1c-804">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="f6d1c-804">Min: `0`</span></span><br><span data-ttu-id="f6d1c-805">最大值： `3600` | |`stdoutLogEnabled` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-805">Max: `3600` | | `stdoutLogEnabled` | </span></span><p><span data-ttu-id="f6d1c-806">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-806">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f6d1c-807">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-807">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> <span data-ttu-id="f6d1c-808">| `false` | | `stdoutLogFile` | </span><span class="sxs-lookup"><span data-stu-id="f6d1c-808">| `false` | | `stdoutLogFile` | </span></span><p><span data-ttu-id="f6d1c-809">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-809">Optional string attribute.</span></span></p><p><span data-ttu-id="f6d1c-810">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-810">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="f6d1c-811">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-811">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="f6d1c-812">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-812">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="f6d1c-813">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-813">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="f6d1c-814">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-814">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="f6d1c-815">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs]\*\* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-815">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="f6d1c-816">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="f6d1c-816">Setting environment variables</span></span>

<span data-ttu-id="f6d1c-817">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-817">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="f6d1c-818">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-818">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="f6d1c-819">此節中所設定的環境變數與使用相同名稱設定的系統環境變數相衝突。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-819">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="f6d1c-820">若同時在 *web.config* 檔案與 Windows 中的系統層級設定環境變數，來自 *web.config* 檔案的值會成為附加到系統環境變數值 (例如，`ASPNETCORE_ENVIRONMENT: Development;Development`)，這會造成應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-820">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="f6d1c-821">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-821">The following example sets two environment variables.</span></span> <span data-ttu-id="f6d1c-822">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-822">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="f6d1c-823">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-823">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="f6d1c-824">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-824">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> <span data-ttu-id="f6d1c-825">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-825">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="f6d1c-826">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="f6d1c-826">app_offline.htm</span></span>

<span data-ttu-id="f6d1c-827">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-827">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="f6d1c-828">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-828">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="f6d1c-829">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-829">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="f6d1c-830">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-830">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="f6d1c-831">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="f6d1c-831">Start-up error page</span></span>

<span data-ttu-id="f6d1c-832">如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗]\*\* 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-832">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="f6d1c-833">若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-833">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="f6d1c-834">如需有關設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤\<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-834">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="f6d1c-836">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="f6d1c-836">Log creation and redirection</span></span>

<span data-ttu-id="f6d1c-837">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-837">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="f6d1c-838">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-838">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="f6d1c-839">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-839">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="f6d1c-840">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-840">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="f6d1c-841">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-841">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="f6d1c-842">僅建議使用 stdout 記錄檔，以便在 IIS 上裝載時或使用[iis Visual Studio 的開發時間支援](xref:host-and-deploy/iis/development-time-iis-support)時，針對應用程式啟動問題進行疑難排解，而不是在本機進行偵錯工具並使用 IIS Express 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-842">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="f6d1c-843">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-843">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="f6d1c-844">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-844">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f6d1c-845">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-845">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="f6d1c-846">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-846">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="f6d1c-847">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 (*.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-847">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="f6d1c-848">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-848">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="f6d1c-849">下列範例 `aspNetCore` 元素會在相對路徑上設定 stdout 記錄 `.\log\` 。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-849">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="f6d1c-850">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-850">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout">
</aspNetCore>
```

<span data-ttu-id="f6d1c-851">發行 Azure App Service 部署的應用程式時，Web SDK 會將 `stdoutLogFile` 值設定為 `\\?\%home%\LogFiles\stdout` 。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-851">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="f6d1c-852">`%home`環境變數是針對 Azure App Service 所主控的應用程式預先定義的。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-852">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="f6d1c-853">若要建立記錄篩選規則，請參閱 ASP.NET Core 記錄檔的[設定和](xref:fundamentals/logging/index#log-filtering)[記錄篩選](xref:fundamentals/logging/index#log-filtering)區段。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-853">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="f6d1c-854">如需路徑格式的詳細資訊，請參閱[Windows 系統上的檔案路徑格式](/dotnet/standard/io/file-path-formats)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-854">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="f6d1c-855">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="f6d1c-855">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="f6d1c-856">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-856">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="f6d1c-857">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-857">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="f6d1c-858">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-858">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="f6d1c-859">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-859">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="f6d1c-860">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-860">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="f6d1c-861">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-861">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="f6d1c-862">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-862">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="f6d1c-863">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-863">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="f6d1c-864">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-864">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="f6d1c-865">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="f6d1c-865">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="f6d1c-866">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-866">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="f6d1c-867">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-867">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="f6d1c-868">使用「IIS 共用設定」時，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-868">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="f6d1c-869">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-869">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="f6d1c-870">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-870">Run the installer.</span></span>
1. <span data-ttu-id="f6d1c-871">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-871">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="f6d1c-872">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-872">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="f6d1c-873">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="f6d1c-873">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="f6d1c-874">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="f6d1c-874">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="f6d1c-875">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-875">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="f6d1c-876">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-876">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="f6d1c-877">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-877">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="f6d1c-878">選取 [**詳細資料**] 索引標籤。檔案**版本**和**產品版本**代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-878">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="f6d1c-879">模組的裝載套件組合安裝程式記錄檔位於*C： \\ Users \\ % UserName% \\ AppData \\ Local \\ Temp*。此檔案的名稱為*dd_DotNetCoreWinSvrHosting__ \<timestamp> _000_AspNetCoreModule_x64 .log*。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-879">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="f6d1c-880">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="f6d1c-880">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="f6d1c-881">模組</span><span class="sxs-lookup"><span data-stu-id="f6d1c-881">Module</span></span>

<span data-ttu-id="f6d1c-882">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-882">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="f6d1c-883">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-883">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="f6d1c-884">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-884">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="f6d1c-885">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-885">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="f6d1c-886">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-886">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="f6d1c-887">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f6d1c-887">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="f6d1c-888">結構描述</span><span class="sxs-lookup"><span data-stu-id="f6d1c-888">Schema</span></span>

<span data-ttu-id="f6d1c-889">**IIS**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-889">**IIS**</span></span>

* <span data-ttu-id="f6d1c-890">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="f6d1c-890">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="f6d1c-891">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-891">**IIS Express**</span></span>

* <span data-ttu-id="f6d1c-892">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="f6d1c-892">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="f6d1c-893">組態</span><span class="sxs-lookup"><span data-stu-id="f6d1c-893">Configuration</span></span>

<span data-ttu-id="f6d1c-894">**IIS**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-894">**IIS**</span></span>

* <span data-ttu-id="f6d1c-895">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="f6d1c-895">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="f6d1c-896">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="f6d1c-896">**IIS Express**</span></span>

* <span data-ttu-id="f6d1c-897">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="f6d1c-897">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="f6d1c-898">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="f6d1c-898">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="f6d1c-899">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-899">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f6d1c-900">其他資源</span><span class="sxs-lookup"><span data-stu-id="f6d1c-900">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <span data-ttu-id="f6d1c-901">[ASP.NET Core 模組參考來源（主要分支）](https://github.com/dotnet/aspnetcore/tree/master/src/Servers/IIS/AspNetCoreModuleV2)：使用 [**分支**] 下拉式清單來選取特定版本（例如， `release/3.1` ）。</span><span class="sxs-lookup"><span data-stu-id="f6d1c-901">[ASP.NET Core Module reference source (master branch)](https://github.com/dotnet/aspnetcore/tree/master/src/Servers/IIS/AspNetCoreModuleV2): Use the **Branch** drop down list to select a specific release (for example, `release/3.1`).</span></span>
* <xref:host-and-deploy/iis/modules>
