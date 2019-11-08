---
title: ASP.NET Core 模組
author: guardrex
description: 了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c9bbd36b8a55b837f6d78abf99215c5496895a39
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799418"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="fa302-103">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="fa302-103">ASP.NET Core Module</span></span>

<span data-ttu-id="fa302-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti)、[Justin Kotalik](https://github.com/jkotalik) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fa302-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fa302-105">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="fa302-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="fa302-106">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="fa302-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="fa302-107">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="fa302-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="fa302-108">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="fa302-108">Supported Windows versions:</span></span>

* <span data-ttu-id="fa302-109">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="fa302-109">Windows 7 or later</span></span>
* <span data-ttu-id="fa302-110">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="fa302-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="fa302-111">同處理序裝載時，模組會使用 IIS 的同處理序伺服程式實作，稱為 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="fa302-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="fa302-112">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="fa302-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="fa302-113">此模組[無法與 HTTP.sys 搭配運作。](xref:fundamentals/servers/httpsys)</span><span class="sxs-lookup"><span data-stu-id="fa302-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="fa302-114">裝載模型</span><span class="sxs-lookup"><span data-stu-id="fa302-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="fa302-115">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="fa302-115">In-process hosting model</span></span>

<span data-ttu-id="fa302-116">ASP.NET Core 應用程式預設為同進程裝載模型。</span><span class="sxs-lookup"><span data-stu-id="fa302-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="fa302-117">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="fa302-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="fa302-118">使用 IIS HTTP 伺服器 (`IISHttpServer`) 而不是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器。</span><span class="sxs-lookup"><span data-stu-id="fa302-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="fa302-119">針對同處理序，[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 來：</span><span class="sxs-lookup"><span data-stu-id="fa302-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="fa302-120">註冊 `IISHttpServer`。</span><span class="sxs-lookup"><span data-stu-id="fa302-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="fa302-121">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="fa302-122">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="fa302-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="fa302-123">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="fa302-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="fa302-124">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="fa302-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="fa302-125">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="fa302-125">Use one app pool per app.</span></span>

* <span data-ttu-id="fa302-126">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="fa302-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="fa302-127">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="fa302-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="fa302-128">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="fa302-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="fa302-129">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="fa302-129">Client disconnects are detected.</span></span> <span data-ttu-id="fa302-130">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="fa302-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="fa302-131">在 ASP.NET Core 2.2.1 或更早版本中，<xref:System.IO.Directory.GetCurrentDirectory*> 會傳回 IIS 所啟動之處理序的背景工作目錄，而非應用程式的目錄 (例如 *w3wp.exe* 為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="fa302-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="fa302-132">如需設定應用程式目前所在目錄的範例程式碼，請參閱 [CurrentDirectoryHelpers 類別](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs)。</span><span class="sxs-lookup"><span data-stu-id="fa302-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="fa302-133">呼叫 `SetCurrentDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="fa302-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="fa302-134">後續呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 會提供應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="fa302-135">裝載同處理序時，不會內部呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 來將使用者初始化。</span><span class="sxs-lookup"><span data-stu-id="fa302-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="fa302-136">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="fa302-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="fa302-137">使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告時，請呼叫 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 來新增入驗證服務：</span><span class="sxs-lookup"><span data-stu-id="fa302-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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
  
  * <span data-ttu-id="fa302-138">不支援[Web 套件（單一檔案）部署](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages)。</span><span class="sxs-lookup"><span data-stu-id="fa302-138">[Web Package (single-file) deployments](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) aren't supported.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="fa302-139">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="fa302-139">Out-of-process hosting model</span></span>

<span data-ttu-id="fa302-140">若要設定跨進程裝載的應用程式，請將 [`<AspNetCoreHostingModel>`] 屬性的值設為專案檔（ *.csproj*）中的 `OutOfProcess`：</span><span class="sxs-lookup"><span data-stu-id="fa302-140">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="fa302-141">同進程裝載是使用 `InProcess` 設定，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="fa302-141">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="fa302-142">`<AspNetCoreHostingModel>` 的值不區分大小寫，因此 `inprocess` 和 `outofprocess` 都是有效的值。</span><span class="sxs-lookup"><span data-stu-id="fa302-142">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="fa302-143">使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器而不是 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="fa302-143">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="fa302-144">針對跨處理序，[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 來：</span><span class="sxs-lookup"><span data-stu-id="fa302-144">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="fa302-145">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-145">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="fa302-146">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="fa302-146">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="fa302-147">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="fa302-147">Hosting model changes</span></span>

<span data-ttu-id="fa302-148">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-148">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="fa302-149">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="fa302-149">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="fa302-150">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-150">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="fa302-151">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="fa302-151">Process name</span></span>

<span data-ttu-id="fa302-152">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="fa302-152">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="fa302-153">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="fa302-153">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="fa302-154">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="fa302-154">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="fa302-155">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="fa302-155">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="fa302-156">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-156">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="fa302-157">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="fa302-157">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="fa302-158">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="fa302-158">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="fa302-159">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="fa302-159">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="fa302-160">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="fa302-160">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="fa302-161">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="fa302-161">Configuration with web.config</span></span>

<span data-ttu-id="fa302-162">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="fa302-162">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="fa302-163">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="fa302-163">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="fa302-164">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="fa302-164">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="fa302-165">將 <xref:System.Configuration.SectionInformation.InheritInChildApplications*> 屬性設定為 `false`，以表示在 [\<位置>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 內指定的設定，不是由位在應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="fa302-165">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="fa302-166">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="fa302-166">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="fa302-167">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="fa302-167">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="fa302-168">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="fa302-168">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="fa302-169">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="fa302-169">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="fa302-170">屬性</span><span class="sxs-lookup"><span data-stu-id="fa302-170">Attribute</span></span> | <span data-ttu-id="fa302-171">描述</span><span class="sxs-lookup"><span data-stu-id="fa302-171">Description</span></span> | <span data-ttu-id="fa302-172">Default</span><span class="sxs-lookup"><span data-stu-id="fa302-172">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="fa302-173">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-173">Optional string attribute.</span></span></p><p><span data-ttu-id="fa302-174">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="fa302-174">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="fa302-175">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-175">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fa302-176">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-176">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="fa302-177">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-177">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fa302-178">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-178">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="fa302-179">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="fa302-179">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="fa302-180">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-180">Optional string attribute.</span></span></p><p><span data-ttu-id="fa302-181">將主控模型指定為同進程（`InProcess`/`inprocess`）或跨進程（`OutOfProcess`/`outofprocess`）。</span><span class="sxs-lookup"><span data-stu-id="fa302-181">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `InProcess`<br>`inprocess` |
| `processesPerApplication` | <p><span data-ttu-id="fa302-182">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-183">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="fa302-183">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="fa302-184">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="fa302-184">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="fa302-185">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="fa302-185">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="fa302-186">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="fa302-186">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="fa302-187">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="fa302-187">Default: `1`</span></span><br><span data-ttu-id="fa302-188">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="fa302-188">Min: `1`</span></span><br><span data-ttu-id="fa302-189">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="fa302-189">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="fa302-190">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-190">Required string attribute.</span></span></p><p><span data-ttu-id="fa302-191">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-191">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="fa302-192">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-192">Relative paths are supported.</span></span> <span data-ttu-id="fa302-193">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-193">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="fa302-194">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-194">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-195">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="fa302-195">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="fa302-196">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-196">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="fa302-197">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="fa302-197">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="fa302-198">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="fa302-198">Default: `10`</span></span><br><span data-ttu-id="fa302-199">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="fa302-199">Min: `0`</span></span><br><span data-ttu-id="fa302-200">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="fa302-200">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="fa302-201">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-201">Optional timespan attribute.</span></span></p><p><span data-ttu-id="fa302-202">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="fa302-202">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="fa302-203">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="fa302-203">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="fa302-204">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="fa302-204">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="fa302-205">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-205">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="fa302-206">字串之分鐘和秒數的有效值介於 0-59。</span><span class="sxs-lookup"><span data-stu-id="fa302-206">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="fa302-207">在分鐘或秒數的值中使用 **60** 將會導致「500 - 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="fa302-207">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="fa302-208">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="fa302-208">Default: `00:02:00`</span></span><br><span data-ttu-id="fa302-209">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="fa302-209">Min: `00:00:00`</span></span><br><span data-ttu-id="fa302-210">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="fa302-210">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="fa302-211">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-211">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-212">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="fa302-212">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="fa302-213">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="fa302-213">Default: `10`</span></span><br><span data-ttu-id="fa302-214">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="fa302-214">Min: `0`</span></span><br><span data-ttu-id="fa302-215">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="fa302-215">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="fa302-216">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-216">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-217">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="fa302-217">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="fa302-218">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-218">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="fa302-219">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="fa302-219">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="fa302-220">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="fa302-220">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="fa302-221">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="fa302-221">Default: `120`</span></span><br><span data-ttu-id="fa302-222">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="fa302-222">Min: `0`</span></span><br><span data-ttu-id="fa302-223">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="fa302-223">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="fa302-224">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-224">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fa302-225">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="fa302-225">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="fa302-226">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-226">Optional string attribute.</span></span></p><p><span data-ttu-id="fa302-227">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-227">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="fa302-228">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-228">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="fa302-229">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-229">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="fa302-230">建立記錄檔後，模組會建立路徑中提供的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="fa302-230">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="fa302-231">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 ( *.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="fa302-231">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="fa302-232">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="fa302-232">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="fa302-233">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="fa302-233">Set environment variables</span></span>

<span data-ttu-id="fa302-234">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-234">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="fa302-235">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-235">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="fa302-236">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-236">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="fa302-237">下列範例會在*web.config 中設定*兩個環境變數。`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="fa302-237">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="fa302-238">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="fa302-238">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="fa302-239">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-239">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="fa302-240">直接在*web.config*中設定環境的替代方法是在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中包含 `<EnvironmentName>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-240">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="fa302-241">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="fa302-241">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="fa302-242">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="fa302-242">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="fa302-243">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="fa302-243">app_offline.htm</span></span>

<span data-ttu-id="fa302-244">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-244">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="fa302-245">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-245">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="fa302-246">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-246">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="fa302-247">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-247">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="fa302-248">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="fa302-248">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="fa302-249">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="fa302-249">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="fa302-250">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="fa302-250">Start-up error page</span></span>

<span data-ttu-id="fa302-251">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-251">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="fa302-252">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-252">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="fa302-253">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-253">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="fa302-254">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-254">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="fa302-255">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-255">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="fa302-256">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="fa302-256">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="fa302-257">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="fa302-257">Log creation and redirection</span></span>

<span data-ttu-id="fa302-258">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="fa302-258">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="fa302-259">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="fa302-259">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="fa302-260">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="fa302-260">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fa302-261">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fa302-261">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="fa302-262">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="fa302-262">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="fa302-263">僅建議使用 stdout 記錄檔，以便在 IIS 上裝載時或使用[iis Visual Studio 的開發時間支援](xref:host-and-deploy/iis/development-time-iis-support)時，針對應用程式啟動問題進行疑難排解，而不是在本機進行偵錯工具並使用 IIS Express 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-263">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="fa302-264">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-264">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="fa302-265">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="fa302-265">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="fa302-266">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="fa302-266">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="fa302-267">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="fa302-267">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="fa302-268">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 ( *.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="fa302-268">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="fa302-269">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="fa302-269">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="fa302-270">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fa302-270">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="fa302-271">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fa302-271">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="fa302-272">下列範例 `aspNetCore` 元素會在相對路徑 `.\log\`設定 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-272">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="fa302-273">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="fa302-273">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="fa302-274">發行 Azure App Service 部署的應用程式時，Web SDK 會將 `stdoutLogFile` 值設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="fa302-274">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="fa302-275">`%home` 環境變數是針對 Azure App Service 所裝載的應用程式預先定義的。</span><span class="sxs-lookup"><span data-stu-id="fa302-275">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="fa302-276">若要建立記錄篩選規則，請參閱 ASP.NET Core 記錄檔的[設定和](xref:fundamentals/logging/index#log-filtering)[記錄篩選](xref:fundamentals/logging/index#log-filtering)區段。</span><span class="sxs-lookup"><span data-stu-id="fa302-276">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="fa302-277">如需路徑格式的詳細資訊，請參閱[Windows 系統上的檔案路徑格式](/dotnet/standard/io/file-path-formats)。</span><span class="sxs-lookup"><span data-stu-id="fa302-277">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="fa302-278">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="fa302-278">Enhanced diagnostic logs</span></span>

<span data-ttu-id="fa302-279">ASP.NET Core 模組是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-279">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="fa302-280">將 `<handlerSettings>` 專案加入*web.config*中的 `<aspNetCore>` 元素。將 `debugLevel` 設定為 `TRACE` 會對診斷資訊公開更高的精確度：</span><span class="sxs-lookup"><span data-stu-id="fa302-280">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="fa302-281">建立記錄檔後，模組會建立路徑 (在上述範例中為 *logs*) 中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="fa302-281">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="fa302-282">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="fa302-282">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fa302-283">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="fa302-283">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="fa302-284">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="fa302-284">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="fa302-285">ERROR</span><span class="sxs-lookup"><span data-stu-id="fa302-285">ERROR</span></span>
* <span data-ttu-id="fa302-286">WARNING</span><span class="sxs-lookup"><span data-stu-id="fa302-286">WARNING</span></span>
* <span data-ttu-id="fa302-287">INFO</span><span class="sxs-lookup"><span data-stu-id="fa302-287">INFO</span></span>
* <span data-ttu-id="fa302-288">TRACE</span><span class="sxs-lookup"><span data-stu-id="fa302-288">TRACE</span></span>

<span data-ttu-id="fa302-289">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="fa302-289">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="fa302-290">主控台</span><span class="sxs-lookup"><span data-stu-id="fa302-290">CONSOLE</span></span>
* <span data-ttu-id="fa302-291">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="fa302-291">EVENTLOG</span></span>
* <span data-ttu-id="fa302-292">檔案</span><span class="sxs-lookup"><span data-stu-id="fa302-292">FILE</span></span>

<span data-ttu-id="fa302-293">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="fa302-293">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="fa302-294">偵錯記錄檔的 `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; 路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-294">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="fa302-295">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="fa302-295">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="fa302-296">`ASPNETCORE_MODULE_DEBUG` &ndash; 偵錯層級設定。</span><span class="sxs-lookup"><span data-stu-id="fa302-296">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="fa302-297">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="fa302-297">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="fa302-298">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="fa302-298">The size of the log isn't limited.</span></span> <span data-ttu-id="fa302-299">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="fa302-299">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="fa302-300">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="fa302-300">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="fa302-301">修改堆疊大小</span><span class="sxs-lookup"><span data-stu-id="fa302-301">Modify the stack size</span></span>

<span data-ttu-id="fa302-302">*僅適用于使用同進程裝載模型時。*</span><span class="sxs-lookup"><span data-stu-id="fa302-302">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="fa302-303">使用 web.config 中的 `stackSize` 設定（以位元組為單位）來設定受控堆疊*大小。* 預設大小為 `1048576` 個位元組（1 MB）。</span><span class="sxs-lookup"><span data-stu-id="fa302-303">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

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

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="fa302-304">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="fa302-304">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="fa302-305">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="fa302-305">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="fa302-306">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="fa302-306">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="fa302-307">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="fa302-307">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="fa302-308">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="fa302-308">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="fa302-309">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="fa302-309">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="fa302-310">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="fa302-310">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="fa302-311">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="fa302-311">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="fa302-312">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-312">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="fa302-313">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="fa302-313">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="fa302-314">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-314">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="fa302-315">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="fa302-315">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="fa302-316">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="fa302-316">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="fa302-317">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fa302-317">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="fa302-318">在與 IIS 安裝相同的電腦上使用 IIS 共用設定時，請執行 ASP.NET Core 裝載套件組合安裝程式並將 `OPT_NO_SHARED_CONFIG_CHECK` 參數設為 `1`：</span><span class="sxs-lookup"><span data-stu-id="fa302-318">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="fa302-319">若共用設定的路徑位於與 IIS 安裝不同的電腦上，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="fa302-319">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="fa302-320">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="fa302-320">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="fa302-321">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-321">Run the installer.</span></span>
1. <span data-ttu-id="fa302-322">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="fa302-322">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="fa302-323">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="fa302-323">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="fa302-324">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="fa302-324">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="fa302-325">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="fa302-325">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="fa302-326">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="fa302-326">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="fa302-327">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="fa302-327">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="fa302-328">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="fa302-328">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="fa302-329">選取 [**詳細資料**] 索引標籤。檔案**版本**和**產品版本**代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="fa302-329">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="fa302-330">模組的裝載套件組合安裝程式記錄檔位於*C：\\使用者\\% UserName%\\AppData\\本機\\Temp*。此檔案的名稱為*dd_DotNetCoreWinSvrHosting__\<時間戳記 > _000_AspNetCoreModule_x64 .log*。</span><span class="sxs-lookup"><span data-stu-id="fa302-330">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="fa302-331">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="fa302-331">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="fa302-332">Module</span><span class="sxs-lookup"><span data-stu-id="fa302-332">Module</span></span>

<span data-ttu-id="fa302-333">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="fa302-333">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="fa302-334">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-334">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="fa302-335">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-335">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="fa302-336">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-336">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="fa302-337">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-337">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="fa302-338">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="fa302-338">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="fa302-339">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-339">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="fa302-340">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-340">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="fa302-341">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-341">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="fa302-342">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-342">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="fa302-343">結構描述</span><span class="sxs-lookup"><span data-stu-id="fa302-343">Schema</span></span>

<span data-ttu-id="fa302-344">**IIS**</span><span class="sxs-lookup"><span data-stu-id="fa302-344">**IIS**</span></span>

* <span data-ttu-id="fa302-345">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fa302-345">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="fa302-346">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="fa302-346">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="fa302-347">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fa302-347">**IIS Express**</span></span>

* <span data-ttu-id="fa302-348">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fa302-348">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="fa302-349">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="fa302-349">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="fa302-350">Configuration</span><span class="sxs-lookup"><span data-stu-id="fa302-350">Configuration</span></span>

<span data-ttu-id="fa302-351">**IIS**</span><span class="sxs-lookup"><span data-stu-id="fa302-351">**IIS**</span></span>

* <span data-ttu-id="fa302-352">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fa302-352">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="fa302-353">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fa302-353">**IIS Express**</span></span>

* <span data-ttu-id="fa302-354">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fa302-354">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="fa302-355">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="fa302-355">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="fa302-356">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="fa302-356">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="fa302-357">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="fa302-357">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="fa302-358">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="fa302-358">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="fa302-359">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="fa302-359">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="fa302-360">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="fa302-360">Supported Windows versions:</span></span>

* <span data-ttu-id="fa302-361">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="fa302-361">Windows 7 or later</span></span>
* <span data-ttu-id="fa302-362">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="fa302-362">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="fa302-363">同處理序裝載時，模組會使用 IIS 的同處理序伺服程式實作，稱為 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="fa302-363">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="fa302-364">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="fa302-364">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="fa302-365">此模組[無法與 HTTP.sys 搭配運作。](xref:fundamentals/servers/httpsys)</span><span class="sxs-lookup"><span data-stu-id="fa302-365">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="fa302-366">裝載模型</span><span class="sxs-lookup"><span data-stu-id="fa302-366">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="fa302-367">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="fa302-367">In-process hosting model</span></span>

<span data-ttu-id="fa302-368">若要設定同處理序裝載的應用程式，請將 `<AspNetCoreHostingModel>` 屬性新增至應用程式的專案檔，其值為 `InProcess` (跨處理序裝載是使用 `OutOfProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="fa302-368">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="fa302-369">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="fa302-369">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="fa302-370">`<AspNetCoreHostingModel>` 的值不區分大小寫，因此 `inprocess` 和 `outofprocess` 都是有效的值。</span><span class="sxs-lookup"><span data-stu-id="fa302-370">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="fa302-371">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="fa302-371">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="fa302-372">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="fa302-372">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="fa302-373">使用 IIS HTTP 伺服器 (`IISHttpServer`) 而不是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器。</span><span class="sxs-lookup"><span data-stu-id="fa302-373">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="fa302-374">針對同處理序，[CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 來：</span><span class="sxs-lookup"><span data-stu-id="fa302-374">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="fa302-375">註冊 `IISHttpServer`。</span><span class="sxs-lookup"><span data-stu-id="fa302-375">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="fa302-376">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-376">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="fa302-377">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="fa302-377">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="fa302-378">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="fa302-378">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="fa302-379">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="fa302-379">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="fa302-380">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="fa302-380">Use one app pool per app.</span></span>

* <span data-ttu-id="fa302-381">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="fa302-381">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="fa302-382">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="fa302-382">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="fa302-383">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="fa302-383">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="fa302-384">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="fa302-384">Client disconnects are detected.</span></span> <span data-ttu-id="fa302-385">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="fa302-385">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="fa302-386">在 ASP.NET Core 2.2.1 或更早版本中，<xref:System.IO.Directory.GetCurrentDirectory*> 會傳回 IIS 所啟動之處理序的背景工作目錄，而非應用程式的目錄 (例如 *w3wp.exe* 為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="fa302-386">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="fa302-387">如需設定應用程式目前所在目錄的範例程式碼，請參閱 [CurrentDirectoryHelpers 類別](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs)。</span><span class="sxs-lookup"><span data-stu-id="fa302-387">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="fa302-388">呼叫 `SetCurrentDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="fa302-388">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="fa302-389">後續呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 會提供應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-389">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="fa302-390">裝載同處理序時，不會內部呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 來將使用者初始化。</span><span class="sxs-lookup"><span data-stu-id="fa302-390">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="fa302-391">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="fa302-391">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="fa302-392">使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告時，請呼叫 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 來新增入驗證服務：</span><span class="sxs-lookup"><span data-stu-id="fa302-392">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="fa302-393">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="fa302-393">Out-of-process hosting model</span></span>

<span data-ttu-id="fa302-394">若要設定跨處理序裝載的應用程式，請在專案檔中使用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="fa302-394">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="fa302-395">請勿指定 `<AspNetCoreHostingModel>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-395">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="fa302-396">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="fa302-396">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="fa302-397">將 `<AspNetCoreHostingModel>` 屬性的值設定為 `OutOfProcess` (同處理序裝載是使用 `InProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="fa302-397">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="fa302-398">此值不區分大小寫，因此 `inprocess` 和 `outofprocess` 都是有效的值。</span><span class="sxs-lookup"><span data-stu-id="fa302-398">The value is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="fa302-399">使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器而不是 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="fa302-399">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="fa302-400">針對跨處理序，[CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 來：</span><span class="sxs-lookup"><span data-stu-id="fa302-400">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="fa302-401">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-401">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="fa302-402">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="fa302-402">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="fa302-403">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="fa302-403">Hosting model changes</span></span>

<span data-ttu-id="fa302-404">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-404">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="fa302-405">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="fa302-405">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="fa302-406">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-406">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="fa302-407">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="fa302-407">Process name</span></span>

<span data-ttu-id="fa302-408">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="fa302-408">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="fa302-409">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="fa302-409">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="fa302-410">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="fa302-410">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="fa302-411">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="fa302-411">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="fa302-412">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-412">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="fa302-413">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="fa302-413">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="fa302-414">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="fa302-414">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="fa302-415">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="fa302-415">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="fa302-416">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="fa302-416">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="fa302-417">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="fa302-417">Configuration with web.config</span></span>

<span data-ttu-id="fa302-418">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="fa302-418">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="fa302-419">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="fa302-419">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="fa302-420">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="fa302-420">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="fa302-421">將 <xref:System.Configuration.SectionInformation.InheritInChildApplications*> 屬性設定為 `false`，以表示在 [\<位置>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 內指定的設定，不是由位在應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="fa302-421">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="fa302-422">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="fa302-422">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="fa302-423">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="fa302-423">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="fa302-424">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="fa302-424">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="fa302-425">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="fa302-425">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="fa302-426">屬性</span><span class="sxs-lookup"><span data-stu-id="fa302-426">Attribute</span></span> | <span data-ttu-id="fa302-427">描述</span><span class="sxs-lookup"><span data-stu-id="fa302-427">Description</span></span> | <span data-ttu-id="fa302-428">Default</span><span class="sxs-lookup"><span data-stu-id="fa302-428">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="fa302-429">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-429">Optional string attribute.</span></span></p><p><span data-ttu-id="fa302-430">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="fa302-430">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="fa302-431">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-431">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fa302-432">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-432">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="fa302-433">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-433">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fa302-434">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-434">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="fa302-435">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="fa302-435">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="fa302-436">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-436">Optional string attribute.</span></span></p><p><span data-ttu-id="fa302-437">將主控模型指定為同進程（`InProcess`/`inprocess`）或跨進程（`OutOfProcess`/`outofprocess`）。</span><span class="sxs-lookup"><span data-stu-id="fa302-437">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `OutOfProcess`<br>`outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="fa302-438">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-438">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-439">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="fa302-439">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="fa302-440">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="fa302-440">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="fa302-441">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="fa302-441">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="fa302-442">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="fa302-442">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="fa302-443">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="fa302-443">Default: `1`</span></span><br><span data-ttu-id="fa302-444">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="fa302-444">Min: `1`</span></span><br><span data-ttu-id="fa302-445">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="fa302-445">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="fa302-446">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-446">Required string attribute.</span></span></p><p><span data-ttu-id="fa302-447">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-447">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="fa302-448">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-448">Relative paths are supported.</span></span> <span data-ttu-id="fa302-449">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-449">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="fa302-450">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-450">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-451">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="fa302-451">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="fa302-452">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-452">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="fa302-453">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="fa302-453">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="fa302-454">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="fa302-454">Default: `10`</span></span><br><span data-ttu-id="fa302-455">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="fa302-455">Min: `0`</span></span><br><span data-ttu-id="fa302-456">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="fa302-456">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="fa302-457">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-457">Optional timespan attribute.</span></span></p><p><span data-ttu-id="fa302-458">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="fa302-458">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="fa302-459">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="fa302-459">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="fa302-460">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="fa302-460">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="fa302-461">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-461">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="fa302-462">字串之分鐘和秒數的有效值介於 0-59。</span><span class="sxs-lookup"><span data-stu-id="fa302-462">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="fa302-463">在分鐘或秒數的值中使用 **60** 將會導致「500 - 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="fa302-463">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="fa302-464">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="fa302-464">Default: `00:02:00`</span></span><br><span data-ttu-id="fa302-465">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="fa302-465">Min: `00:00:00`</span></span><br><span data-ttu-id="fa302-466">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="fa302-466">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="fa302-467">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-467">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-468">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="fa302-468">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="fa302-469">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="fa302-469">Default: `10`</span></span><br><span data-ttu-id="fa302-470">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="fa302-470">Min: `0`</span></span><br><span data-ttu-id="fa302-471">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="fa302-471">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="fa302-472">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-472">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-473">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="fa302-473">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="fa302-474">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-474">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="fa302-475">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="fa302-475">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="fa302-476">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="fa302-476">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="fa302-477">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="fa302-477">Default: `120`</span></span><br><span data-ttu-id="fa302-478">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="fa302-478">Min: `0`</span></span><br><span data-ttu-id="fa302-479">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="fa302-479">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="fa302-480">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-480">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fa302-481">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="fa302-481">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="fa302-482">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-482">Optional string attribute.</span></span></p><p><span data-ttu-id="fa302-483">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-483">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="fa302-484">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-484">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="fa302-485">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-485">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="fa302-486">建立記錄檔後，模組會建立路徑中提供的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="fa302-486">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="fa302-487">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 ( *.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="fa302-487">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="fa302-488">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="fa302-488">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="fa302-489">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="fa302-489">Setting environment variables</span></span>

<span data-ttu-id="fa302-490">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-490">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="fa302-491">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-491">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="fa302-492">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-492">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="fa302-493">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-493">The following example sets two environment variables.</span></span> <span data-ttu-id="fa302-494">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="fa302-494">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="fa302-495">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="fa302-495">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="fa302-496">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-496">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="fa302-497">直接在*web.config*中設定環境的替代方法是在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中包含 `<EnvironmentName>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-497">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="fa302-498">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="fa302-498">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="fa302-499">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="fa302-499">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="fa302-500">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="fa302-500">app_offline.htm</span></span>

<span data-ttu-id="fa302-501">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-501">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="fa302-502">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-502">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="fa302-503">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-503">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="fa302-504">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-504">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="fa302-505">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="fa302-505">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="fa302-506">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="fa302-506">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="fa302-507">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="fa302-507">Start-up error page</span></span>

<span data-ttu-id="fa302-508">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-508">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="fa302-509">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-509">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="fa302-510">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-510">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="fa302-511">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-511">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="fa302-512">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-512">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="fa302-513">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="fa302-513">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="fa302-514">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="fa302-514">Log creation and redirection</span></span>

<span data-ttu-id="fa302-515">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="fa302-515">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="fa302-516">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="fa302-516">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="fa302-517">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="fa302-517">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fa302-518">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fa302-518">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="fa302-519">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="fa302-519">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="fa302-520">僅建議使用 stdout 記錄檔，以便在 IIS 上裝載時或使用[iis Visual Studio 的開發時間支援](xref:host-and-deploy/iis/development-time-iis-support)時，針對應用程式啟動問題進行疑難排解，而不是在本機進行偵錯工具並使用 IIS Express 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-520">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="fa302-521">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-521">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="fa302-522">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="fa302-522">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="fa302-523">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="fa302-523">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="fa302-524">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="fa302-524">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="fa302-525">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 ( *.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="fa302-525">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="fa302-526">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="fa302-526">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="fa302-527">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fa302-527">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="fa302-528">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fa302-528">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="fa302-529">下列範例 `aspNetCore` 元素會在相對路徑 `.\log\`設定 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-529">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="fa302-530">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="fa302-530">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="fa302-531">發行 Azure App Service 部署的應用程式時，Web SDK 會將 `stdoutLogFile` 值設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="fa302-531">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="fa302-532">`%home` 環境變數是針對 Azure App Service 所裝載的應用程式預先定義的。</span><span class="sxs-lookup"><span data-stu-id="fa302-532">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="fa302-533">如需路徑格式的詳細資訊，請參閱[Windows 系統上的檔案路徑格式](/dotnet/standard/io/file-path-formats)。</span><span class="sxs-lookup"><span data-stu-id="fa302-533">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="fa302-534">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="fa302-534">Enhanced diagnostic logs</span></span>

<span data-ttu-id="fa302-535">ASP.NET Core 模組是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-535">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="fa302-536">將 `<handlerSettings>` 專案加入*web.config*中的 `<aspNetCore>` 元素。將 `debugLevel` 設定為 `TRACE` 會對診斷資訊公開更高的精確度：</span><span class="sxs-lookup"><span data-stu-id="fa302-536">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="fa302-537">模組不會在提供給 `<handlerSetting>` 值的路徑 (在上述範例中為 *logs*) 中自動建立資料夾，而且這些資料夾應該已存在於部署中。</span><span class="sxs-lookup"><span data-stu-id="fa302-537">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="fa302-538">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="fa302-538">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fa302-539">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="fa302-539">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="fa302-540">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="fa302-540">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="fa302-541">ERROR</span><span class="sxs-lookup"><span data-stu-id="fa302-541">ERROR</span></span>
* <span data-ttu-id="fa302-542">WARNING</span><span class="sxs-lookup"><span data-stu-id="fa302-542">WARNING</span></span>
* <span data-ttu-id="fa302-543">INFO</span><span class="sxs-lookup"><span data-stu-id="fa302-543">INFO</span></span>
* <span data-ttu-id="fa302-544">TRACE</span><span class="sxs-lookup"><span data-stu-id="fa302-544">TRACE</span></span>

<span data-ttu-id="fa302-545">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="fa302-545">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="fa302-546">主控台</span><span class="sxs-lookup"><span data-stu-id="fa302-546">CONSOLE</span></span>
* <span data-ttu-id="fa302-547">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="fa302-547">EVENTLOG</span></span>
* <span data-ttu-id="fa302-548">檔案</span><span class="sxs-lookup"><span data-stu-id="fa302-548">FILE</span></span>

<span data-ttu-id="fa302-549">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="fa302-549">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="fa302-550">偵錯記錄檔的 `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; 路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-550">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="fa302-551">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="fa302-551">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="fa302-552">`ASPNETCORE_MODULE_DEBUG` &ndash; 偵錯層級設定。</span><span class="sxs-lookup"><span data-stu-id="fa302-552">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="fa302-553">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="fa302-553">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="fa302-554">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="fa302-554">The size of the log isn't limited.</span></span> <span data-ttu-id="fa302-555">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="fa302-555">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="fa302-556">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="fa302-556">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="fa302-557">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="fa302-557">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="fa302-558">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="fa302-558">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="fa302-559">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="fa302-559">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="fa302-560">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="fa302-560">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="fa302-561">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="fa302-561">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="fa302-562">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="fa302-562">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="fa302-563">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="fa302-563">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="fa302-564">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="fa302-564">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="fa302-565">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-565">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="fa302-566">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="fa302-566">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="fa302-567">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-567">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="fa302-568">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="fa302-568">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="fa302-569">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="fa302-569">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="fa302-570">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fa302-570">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="fa302-571">在與 IIS 安裝相同的電腦上使用 IIS 共用設定時，請執行 ASP.NET Core 裝載套件組合安裝程式並將 `OPT_NO_SHARED_CONFIG_CHECK` 參數設為 `1`：</span><span class="sxs-lookup"><span data-stu-id="fa302-571">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="fa302-572">若共用設定的路徑位於與 IIS 安裝不同的電腦上，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="fa302-572">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="fa302-573">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="fa302-573">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="fa302-574">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-574">Run the installer.</span></span>
1. <span data-ttu-id="fa302-575">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="fa302-575">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="fa302-576">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="fa302-576">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="fa302-577">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="fa302-577">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="fa302-578">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="fa302-578">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="fa302-579">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="fa302-579">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="fa302-580">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="fa302-580">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="fa302-581">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="fa302-581">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="fa302-582">選取 [**詳細資料**] 索引標籤。檔案**版本**和**產品版本**代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="fa302-582">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="fa302-583">模組的裝載套件組合安裝程式記錄檔位於*C：\\使用者\\% UserName%\\AppData\\本機\\Temp*。此檔案的名稱為*dd_DotNetCoreWinSvrHosting__\<時間戳記 > _000_AspNetCoreModule_x64 .log*。</span><span class="sxs-lookup"><span data-stu-id="fa302-583">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="fa302-584">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="fa302-584">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="fa302-585">Module</span><span class="sxs-lookup"><span data-stu-id="fa302-585">Module</span></span>

<span data-ttu-id="fa302-586">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="fa302-586">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="fa302-587">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-587">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="fa302-588">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-588">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="fa302-589">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-589">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="fa302-590">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-590">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="fa302-591">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="fa302-591">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="fa302-592">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-592">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="fa302-593">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-593">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="fa302-594">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-594">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="fa302-595">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-595">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="fa302-596">結構描述</span><span class="sxs-lookup"><span data-stu-id="fa302-596">Schema</span></span>

<span data-ttu-id="fa302-597">**IIS**</span><span class="sxs-lookup"><span data-stu-id="fa302-597">**IIS**</span></span>

* <span data-ttu-id="fa302-598">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fa302-598">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="fa302-599">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="fa302-599">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="fa302-600">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fa302-600">**IIS Express**</span></span>

* <span data-ttu-id="fa302-601">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fa302-601">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="fa302-602">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="fa302-602">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="fa302-603">Configuration</span><span class="sxs-lookup"><span data-stu-id="fa302-603">Configuration</span></span>

<span data-ttu-id="fa302-604">**IIS**</span><span class="sxs-lookup"><span data-stu-id="fa302-604">**IIS**</span></span>

* <span data-ttu-id="fa302-605">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fa302-605">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="fa302-606">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fa302-606">**IIS Express**</span></span>

* <span data-ttu-id="fa302-607">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fa302-607">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="fa302-608">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="fa302-608">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="fa302-609">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="fa302-609">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fa302-610">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線，將 Web 要求重新轉送到後端的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-610">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="fa302-611">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="fa302-611">Supported Windows versions:</span></span>

* <span data-ttu-id="fa302-612">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="fa302-612">Windows 7 or later</span></span>
* <span data-ttu-id="fa302-613">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="fa302-613">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="fa302-614">該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="fa302-614">The module only works with Kestrel.</span></span> <span data-ttu-id="fa302-615">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) 不相容。</span><span class="sxs-lookup"><span data-stu-id="fa302-615">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="fa302-616">因為處理序中執行的 ASP.NET Core 應用程式會與 IIS 背景工作處理序分開，所以此模組也會執行處理序管理。</span><span class="sxs-lookup"><span data-stu-id="fa302-616">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="fa302-617">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="fa302-617">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="fa302-618">此行為基本上與在 IIS 中執行同處理序，並由 [Windows 處理器啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的 ASP.NET 4.x 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="fa302-618">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="fa302-619">下圖說明 IIS、ASP.NET Core 模組和應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="fa302-619">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core 模組](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="fa302-621">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-621">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="fa302-622">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="fa302-622">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="fa302-623">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="fa302-623">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="fa302-624">此模組在啟動時透過環境變數指定連接埠，而 [IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="fa302-624">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="fa302-625">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="fa302-625">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="fa302-626">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="fa302-626">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="fa302-627">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="fa302-627">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="fa302-628">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="fa302-628">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="fa302-629">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="fa302-629">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="fa302-630">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="fa302-630">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="fa302-631">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="fa302-631">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="fa302-632">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="fa302-632">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="fa302-633">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="fa302-633">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="fa302-634">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-634">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="fa302-635">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="fa302-635">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="fa302-636">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="fa302-636">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="fa302-637">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="fa302-637">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="fa302-638">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="fa302-638">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="fa302-639">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="fa302-639">Configuration with web.config</span></span>

<span data-ttu-id="fa302-640">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="fa302-640">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="fa302-641">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="fa302-641">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="fa302-642">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="fa302-642">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="fa302-643">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="fa302-643">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="fa302-644">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="fa302-644">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="fa302-645">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="fa302-645">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="fa302-646">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="fa302-646">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="fa302-647">屬性</span><span class="sxs-lookup"><span data-stu-id="fa302-647">Attribute</span></span> | <span data-ttu-id="fa302-648">描述</span><span class="sxs-lookup"><span data-stu-id="fa302-648">Description</span></span> | <span data-ttu-id="fa302-649">Default</span><span class="sxs-lookup"><span data-stu-id="fa302-649">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="fa302-650">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-650">Optional string attribute.</span></span></p><p><span data-ttu-id="fa302-651">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="fa302-651">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="fa302-652">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-652">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fa302-653">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-653">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="fa302-654">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-654">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fa302-655">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-655">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="fa302-656">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="fa302-656">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="fa302-657">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-657">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-658">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="fa302-658">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="fa302-659">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="fa302-659">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="fa302-660">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="fa302-660">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="fa302-661">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="fa302-661">Default: `1`</span></span><br><span data-ttu-id="fa302-662">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="fa302-662">Min: `1`</span></span><br><span data-ttu-id="fa302-663">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="fa302-663">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="fa302-664">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-664">Required string attribute.</span></span></p><p><span data-ttu-id="fa302-665">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-665">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="fa302-666">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-666">Relative paths are supported.</span></span> <span data-ttu-id="fa302-667">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-667">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="fa302-668">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-668">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-669">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="fa302-669">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="fa302-670">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-670">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="fa302-671">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="fa302-671">Default: `10`</span></span><br><span data-ttu-id="fa302-672">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="fa302-672">Min: `0`</span></span><br><span data-ttu-id="fa302-673">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="fa302-673">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="fa302-674">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-674">Optional timespan attribute.</span></span></p><p><span data-ttu-id="fa302-675">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="fa302-675">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="fa302-676">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="fa302-676">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="fa302-677">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="fa302-677">Default: `00:02:00`</span></span><br><span data-ttu-id="fa302-678">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="fa302-678">Min: `00:00:00`</span></span><br><span data-ttu-id="fa302-679">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="fa302-679">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="fa302-680">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-680">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-681">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="fa302-681">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="fa302-682">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="fa302-682">Default: `10`</span></span><br><span data-ttu-id="fa302-683">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="fa302-683">Min: `0`</span></span><br><span data-ttu-id="fa302-684">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="fa302-684">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="fa302-685">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-685">Optional integer attribute.</span></span></p><p><span data-ttu-id="fa302-686">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="fa302-686">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="fa302-687">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-687">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="fa302-688">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="fa302-688">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="fa302-689">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="fa302-689">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="fa302-690">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="fa302-690">Default: `120`</span></span><br><span data-ttu-id="fa302-691">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="fa302-691">Min: `0`</span></span><br><span data-ttu-id="fa302-692">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="fa302-692">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="fa302-693">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-693">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="fa302-694">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="fa302-694">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="fa302-695">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-695">Optional string attribute.</span></span></p><p><span data-ttu-id="fa302-696">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-696">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="fa302-697">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-697">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="fa302-698">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-698">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="fa302-699">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fa302-699">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="fa302-700">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 ( *.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="fa302-700">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="fa302-701">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="fa302-701">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="fa302-702">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="fa302-702">Setting environment variables</span></span>

<span data-ttu-id="fa302-703">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-703">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="fa302-704">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-704">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="fa302-705">此節中所設定的環境變數與使用相同名稱設定的系統環境變數相衝突。</span><span class="sxs-lookup"><span data-stu-id="fa302-705">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="fa302-706">若同時在 *web.config* 檔案與 Windows 中的系統層級設定環境變數，來自 *web.config* 檔案的值會成為附加到系統環境變數值 (例如，`ASPNETCORE_ENVIRONMENT: Development;Development`)，這會造成應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="fa302-706">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="fa302-707">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="fa302-707">The following example sets two environment variables.</span></span> <span data-ttu-id="fa302-708">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="fa302-708">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="fa302-709">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="fa302-709">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="fa302-710">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="fa302-710">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="fa302-711">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="fa302-711">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="fa302-712">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="fa302-712">app_offline.htm</span></span>

<span data-ttu-id="fa302-713">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-713">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="fa302-714">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="fa302-714">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="fa302-715">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-715">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="fa302-716">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-716">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="fa302-717">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="fa302-717">Start-up error page</span></span>

<span data-ttu-id="fa302-718">如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="fa302-718">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="fa302-719">若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="fa302-719">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="fa302-720">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="fa302-720">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="fa302-722">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="fa302-722">Log creation and redirection</span></span>

<span data-ttu-id="fa302-723">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="fa302-723">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="fa302-724">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="fa302-724">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="fa302-725">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="fa302-725">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="fa302-726">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fa302-726">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="fa302-727">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="fa302-727">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="fa302-728">僅建議使用 stdout 記錄檔，以便在 IIS 上裝載時或使用[iis Visual Studio 的開發時間支援](xref:host-and-deploy/iis/development-time-iis-support)時，針對應用程式啟動問題進行疑難排解，而不是在本機進行偵錯工具並使用 IIS Express 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-728">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="fa302-729">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-729">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="fa302-730">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="fa302-730">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="fa302-731">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="fa302-731">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="fa302-732">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="fa302-732">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="fa302-733">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 ( *.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="fa302-733">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="fa302-734">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="fa302-734">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="fa302-735">下列範例 `aspNetCore` 元素會在相對路徑 `.\log\`設定 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="fa302-735">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="fa302-736">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="fa302-736">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout">
</aspNetCore>
```

<span data-ttu-id="fa302-737">發行 Azure App Service 部署的應用程式時，Web SDK 會將 `stdoutLogFile` 值設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="fa302-737">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="fa302-738">`%home` 環境變數是針對 Azure App Service 所裝載的應用程式預先定義的。</span><span class="sxs-lookup"><span data-stu-id="fa302-738">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="fa302-739">若要建立記錄篩選規則，請參閱 ASP.NET Core 記錄檔的[設定和](xref:fundamentals/logging/index#log-filtering)[記錄篩選](xref:fundamentals/logging/index#log-filtering)區段。</span><span class="sxs-lookup"><span data-stu-id="fa302-739">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="fa302-740">如需路徑格式的詳細資訊，請參閱[Windows 系統上的檔案路徑格式](/dotnet/standard/io/file-path-formats)。</span><span class="sxs-lookup"><span data-stu-id="fa302-740">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="fa302-741">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="fa302-741">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="fa302-742">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="fa302-742">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="fa302-743">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="fa302-743">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="fa302-744">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="fa302-744">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="fa302-745">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="fa302-745">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="fa302-746">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="fa302-746">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="fa302-747">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="fa302-747">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="fa302-748">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-748">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="fa302-749">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="fa302-749">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="fa302-750">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="fa302-750">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="fa302-751">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="fa302-751">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="fa302-752">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="fa302-752">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="fa302-753">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fa302-753">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="fa302-754">使用「IIS 共用設定」時，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="fa302-754">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="fa302-755">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="fa302-755">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="fa302-756">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="fa302-756">Run the installer.</span></span>
1. <span data-ttu-id="fa302-757">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="fa302-757">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="fa302-758">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="fa302-758">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="fa302-759">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="fa302-759">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="fa302-760">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="fa302-760">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="fa302-761">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="fa302-761">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="fa302-762">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="fa302-762">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="fa302-763">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="fa302-763">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="fa302-764">選取 [**詳細資料**] 索引標籤。檔案**版本**和**產品版本**代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="fa302-764">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="fa302-765">模組的裝載套件組合安裝程式記錄檔位於*C：\\使用者\\% UserName%\\AppData\\本機\\Temp*。此檔案的名稱為*dd_DotNetCoreWinSvrHosting__\<時間戳記 > _000_AspNetCoreModule_x64 .log*。</span><span class="sxs-lookup"><span data-stu-id="fa302-765">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="fa302-766">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="fa302-766">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="fa302-767">Module</span><span class="sxs-lookup"><span data-stu-id="fa302-767">Module</span></span>

<span data-ttu-id="fa302-768">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="fa302-768">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="fa302-769">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-769">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="fa302-770">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-770">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="fa302-771">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="fa302-771">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="fa302-772">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-772">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="fa302-773">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="fa302-773">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="fa302-774">結構描述</span><span class="sxs-lookup"><span data-stu-id="fa302-774">Schema</span></span>

<span data-ttu-id="fa302-775">**IIS**</span><span class="sxs-lookup"><span data-stu-id="fa302-775">**IIS**</span></span>

* <span data-ttu-id="fa302-776">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fa302-776">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="fa302-777">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fa302-777">**IIS Express**</span></span>

* <span data-ttu-id="fa302-778">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="fa302-778">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="fa302-779">Configuration</span><span class="sxs-lookup"><span data-stu-id="fa302-779">Configuration</span></span>

<span data-ttu-id="fa302-780">**IIS**</span><span class="sxs-lookup"><span data-stu-id="fa302-780">**IIS**</span></span>

* <span data-ttu-id="fa302-781">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fa302-781">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="fa302-782">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="fa302-782">**IIS Express**</span></span>

* <span data-ttu-id="fa302-783">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="fa302-783">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="fa302-784">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="fa302-784">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="fa302-785">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="fa302-785">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fa302-786">其他資源</span><span class="sxs-lookup"><span data-stu-id="fa302-786">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="fa302-787">ASP.NET Core 模組 GitHub 存放庫 (參考來源)</span><span class="sxs-lookup"><span data-stu-id="fa302-787">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
