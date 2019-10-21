---
title: ASP.NET Core 模組
author: guardrex
description: 了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/13/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 917ee462a8f9592120685b53d059a661cb4a7452
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333898"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="e1e86-103">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="e1e86-103">ASP.NET Core Module</span></span>

<span data-ttu-id="e1e86-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti)、[Justin Kotalik](https://github.com/jkotalik) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e1e86-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e1e86-105">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="e1e86-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="e1e86-106">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="e1e86-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="e1e86-107">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="e1e86-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="e1e86-108">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="e1e86-108">Supported Windows versions:</span></span>

* <span data-ttu-id="e1e86-109">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e1e86-109">Windows 7 or later</span></span>
* <span data-ttu-id="e1e86-110">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e1e86-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="e1e86-111">同處理序裝載時，模組會使用 IIS 的同處理序伺服程式實作，稱為 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="e1e86-112">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="e1e86-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="e1e86-113">此模組[無法與 HTTP.sys 搭配運作。](xref:fundamentals/servers/httpsys)</span><span class="sxs-lookup"><span data-stu-id="e1e86-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="e1e86-114">裝載模型</span><span class="sxs-lookup"><span data-stu-id="e1e86-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="e1e86-115">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="e1e86-115">In-process hosting model</span></span>

<span data-ttu-id="e1e86-116">ASP.NET Core 應用程式預設為同進程裝載模型。</span><span class="sxs-lookup"><span data-stu-id="e1e86-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="e1e86-117">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="e1e86-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="e1e86-118">使用 IIS HTTP 伺服器 (`IISHttpServer`) 而不是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器。</span><span class="sxs-lookup"><span data-stu-id="e1e86-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="e1e86-119">針對同處理序，[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 來：</span><span class="sxs-lookup"><span data-stu-id="e1e86-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="e1e86-120">註冊 `IISHttpServer`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="e1e86-121">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="e1e86-122">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="e1e86-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="e1e86-123">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="e1e86-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="e1e86-124">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="e1e86-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="e1e86-125">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="e1e86-125">Use one app pool per app.</span></span>

* <span data-ttu-id="e1e86-126">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="e1e86-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="e1e86-127">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="e1e86-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="e1e86-128">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="e1e86-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="e1e86-129">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="e1e86-129">Client disconnects are detected.</span></span> <span data-ttu-id="e1e86-130">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="e1e86-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="e1e86-131">在 ASP.NET Core 2.2.1 或更早版本中，<xref:System.IO.Directory.GetCurrentDirectory*> 會傳回 IIS 所啟動之處理序的背景工作目錄，而非應用程式的目錄 (例如 *w3wp.exe* 為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="e1e86-132">如需設定應用程式目前所在目錄的範例程式碼，請參閱 [CurrentDirectoryHelpers 類別](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="e1e86-133">呼叫 `SetCurrentDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="e1e86-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="e1e86-134">後續呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 會提供應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="e1e86-135">裝載同處理序時，不會內部呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 來將使用者初始化。</span><span class="sxs-lookup"><span data-stu-id="e1e86-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="e1e86-136">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="e1e86-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="e1e86-137">使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告時，請呼叫 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 來新增入驗證服務：</span><span class="sxs-lookup"><span data-stu-id="e1e86-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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
  
  * <span data-ttu-id="e1e86-138">不支援[Web 套件（單一檔案）部署](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-138">[Web Package (single-file) deployments](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) aren't supported.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="e1e86-139">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="e1e86-139">Out-of-process hosting model</span></span>

<span data-ttu-id="e1e86-140">若要設定跨進程裝載的應用程式，請將 [`<AspNetCoreHostingModel>`] 屬性的值設為專案檔（ *.csproj*）中的 `OutOfProcess`：</span><span class="sxs-lookup"><span data-stu-id="e1e86-140">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="e1e86-141">同進程裝載是使用 `InProcess` 設定，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="e1e86-141">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="e1e86-142">使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器而不是 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-142">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="e1e86-143">針對跨處理序，[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 來：</span><span class="sxs-lookup"><span data-stu-id="e1e86-143">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="e1e86-144">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-144">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="e1e86-145">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="e1e86-145">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="e1e86-146">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="e1e86-146">Hosting model changes</span></span>

<span data-ttu-id="e1e86-147">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-147">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="e1e86-148">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="e1e86-148">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="e1e86-149">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-149">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="e1e86-150">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="e1e86-150">Process name</span></span>

<span data-ttu-id="e1e86-151">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-151">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="e1e86-152">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="e1e86-152">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="e1e86-153">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="e1e86-153">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="e1e86-154">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="e1e86-154">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="e1e86-155">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-155">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="e1e86-156">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="e1e86-156">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="e1e86-157">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="e1e86-157">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="e1e86-158">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="e1e86-158">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="e1e86-159">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-159">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="e1e86-160">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="e1e86-160">Configuration with web.config</span></span>

<span data-ttu-id="e1e86-161">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-161">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="e1e86-162">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="e1e86-162">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="e1e86-163">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="e1e86-163">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="e1e86-164">將 <xref:System.Configuration.SectionInformation.InheritInChildApplications*> 屬性設定為 `false`，以表示在 [\<位置>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 內指定的設定，不是由位在應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="e1e86-164">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="e1e86-165">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-165">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="e1e86-166">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="e1e86-166">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="e1e86-167">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="e1e86-167">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="e1e86-168">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="e1e86-168">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="e1e86-169">屬性</span><span class="sxs-lookup"><span data-stu-id="e1e86-169">Attribute</span></span> | <span data-ttu-id="e1e86-170">描述</span><span class="sxs-lookup"><span data-stu-id="e1e86-170">Description</span></span> | <span data-ttu-id="e1e86-171">Default</span><span class="sxs-lookup"><span data-stu-id="e1e86-171">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="e1e86-172">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-172">Optional string attribute.</span></span></p><p><span data-ttu-id="e1e86-173">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-173">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="e1e86-174">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-174">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="e1e86-175">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-175">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="e1e86-176">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-176">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="e1e86-177">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-177">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="e1e86-178">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="e1e86-178">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="e1e86-179">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-179">Optional string attribute.</span></span></p><p><span data-ttu-id="e1e86-180">將裝載模型指定為同處理序 (`InProcess`) 或跨處理序 (`OutOfProcess`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-180">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `InProcess` |
| `processesPerApplication` | <p><span data-ttu-id="e1e86-181">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-182">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="e1e86-182">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="e1e86-183">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-183">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="e1e86-184">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-184">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="e1e86-185">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="e1e86-185">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="e1e86-186">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="e1e86-186">Default: `1`</span></span><br><span data-ttu-id="e1e86-187">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="e1e86-187">Min: `1`</span></span><br><span data-ttu-id="e1e86-188">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="e1e86-188">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="e1e86-189">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-189">Required string attribute.</span></span></p><p><span data-ttu-id="e1e86-190">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-190">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="e1e86-191">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-191">Relative paths are supported.</span></span> <span data-ttu-id="e1e86-192">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-192">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="e1e86-193">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-193">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-194">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-194">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="e1e86-195">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-195">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="e1e86-196">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="e1e86-196">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="e1e86-197">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="e1e86-197">Default: `10`</span></span><br><span data-ttu-id="e1e86-198">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="e1e86-198">Min: `0`</span></span><br><span data-ttu-id="e1e86-199">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="e1e86-199">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="e1e86-200">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-200">Optional timespan attribute.</span></span></p><p><span data-ttu-id="e1e86-201">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="e1e86-201">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="e1e86-202">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-202">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="e1e86-203">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="e1e86-203">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="e1e86-204">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-204">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="e1e86-205">字串之分鐘和秒數的有效值介於 0-59。</span><span class="sxs-lookup"><span data-stu-id="e1e86-205">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="e1e86-206">在分鐘或秒數的值中使用 **60** 將會導致「500 - 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="e1e86-206">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="e1e86-207">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="e1e86-207">Default: `00:02:00`</span></span><br><span data-ttu-id="e1e86-208">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="e1e86-208">Min: `00:00:00`</span></span><br><span data-ttu-id="e1e86-209">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="e1e86-209">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="e1e86-210">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-210">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-211">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-211">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="e1e86-212">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="e1e86-212">Default: `10`</span></span><br><span data-ttu-id="e1e86-213">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="e1e86-213">Min: `0`</span></span><br><span data-ttu-id="e1e86-214">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="e1e86-214">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="e1e86-215">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-215">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-216">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-216">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="e1e86-217">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-217">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="e1e86-218">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-218">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="e1e86-219">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="e1e86-219">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="e1e86-220">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="e1e86-220">Default: `120`</span></span><br><span data-ttu-id="e1e86-221">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="e1e86-221">Min: `0`</span></span><br><span data-ttu-id="e1e86-222">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="e1e86-222">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="e1e86-223">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-223">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="e1e86-224">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="e1e86-224">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="e1e86-225">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-225">Optional string attribute.</span></span></p><p><span data-ttu-id="e1e86-226">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-226">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="e1e86-227">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-227">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="e1e86-228">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-228">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="e1e86-229">建立記錄檔後，模組會建立路徑中提供的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="e1e86-229">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="e1e86-230">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 ( *.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="e1e86-230">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="e1e86-231">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e1e86-231">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="e1e86-232">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="e1e86-232">Set environment variables</span></span>

<span data-ttu-id="e1e86-233">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-233">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="e1e86-234">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-234">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="e1e86-235">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-235">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="e1e86-236">下列範例會在*web.config 中設定*兩個環境變數。 `ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-236">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="e1e86-237">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="e1e86-237">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="e1e86-238">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-238">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> <span data-ttu-id="e1e86-239">在 *web.config* 中直接設定環境的替代方式是在發行設定檔 ( *.pubxml*) 或專案檔中包括 `<EnvironmentName>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-239">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="e1e86-240">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="e1e86-240">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="e1e86-241">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-241">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="e1e86-242">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="e1e86-242">app_offline.htm</span></span>

<span data-ttu-id="e1e86-243">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-243">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="e1e86-244">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-244">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="e1e86-245">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-245">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="e1e86-246">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e1e86-246">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="e1e86-247">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="e1e86-247">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="e1e86-248">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="e1e86-248">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="e1e86-249">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="e1e86-249">Start-up error page</span></span>

<span data-ttu-id="e1e86-250">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-250">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="e1e86-251">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-251">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="e1e86-252">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-252">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="e1e86-253">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-253">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="e1e86-254">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-254">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="e1e86-255">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-255">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="e1e86-256">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="e1e86-256">Log creation and redirection</span></span>

<span data-ttu-id="e1e86-257">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="e1e86-257">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="e1e86-258">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="e1e86-258">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="e1e86-259">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-259">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="e1e86-260">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-260">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="e1e86-261">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="e1e86-261">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="e1e86-262">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-262">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="e1e86-263">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-263">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="e1e86-264">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="e1e86-264">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="e1e86-265">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-265">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="e1e86-266">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="e1e86-266">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="e1e86-267">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 ( *.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="e1e86-267">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="e1e86-268">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="e1e86-268">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="e1e86-269">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-269">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="e1e86-270">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-270">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="e1e86-271">下列範例 *`aspNetCore` web.config 檔案*中的元素會針對裝載于 Azure App Service 中的應用程式，設定 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-271">The following sample `aspNetCore` element in a *web.config* file configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="e1e86-272">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-272">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="e1e86-273">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="e1e86-273">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="e1e86-274">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="e1e86-274">Enhanced diagnostic logs</span></span>

<span data-ttu-id="e1e86-275">ASP.NET Core 模組是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-275">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="e1e86-276">將 `<handlerSettings>` 專案加入*web.config*中的 `<aspNetCore>` 元素。將 `debugLevel` 設定為 `TRACE` 會對診斷資訊公開更高的精確度：</span><span class="sxs-lookup"><span data-stu-id="e1e86-276">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="e1e86-277">建立記錄檔後，模組會建立路徑 (在上述範例中為 *logs*) 中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="e1e86-277">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="e1e86-278">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-278">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="e1e86-279">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="e1e86-279">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="e1e86-280">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="e1e86-280">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="e1e86-281">ERROR</span><span class="sxs-lookup"><span data-stu-id="e1e86-281">ERROR</span></span>
* <span data-ttu-id="e1e86-282">WARNING</span><span class="sxs-lookup"><span data-stu-id="e1e86-282">WARNING</span></span>
* <span data-ttu-id="e1e86-283">INFO</span><span class="sxs-lookup"><span data-stu-id="e1e86-283">INFO</span></span>
* <span data-ttu-id="e1e86-284">TRACE</span><span class="sxs-lookup"><span data-stu-id="e1e86-284">TRACE</span></span>

<span data-ttu-id="e1e86-285">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="e1e86-285">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="e1e86-286">主控台</span><span class="sxs-lookup"><span data-stu-id="e1e86-286">CONSOLE</span></span>
* <span data-ttu-id="e1e86-287">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="e1e86-287">EVENTLOG</span></span>
* <span data-ttu-id="e1e86-288">檔案</span><span class="sxs-lookup"><span data-stu-id="e1e86-288">FILE</span></span>

<span data-ttu-id="e1e86-289">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="e1e86-289">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="e1e86-290">偵錯記錄檔的 `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; 路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-290">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="e1e86-291">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="e1e86-291">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="e1e86-292">`ASPNETCORE_MODULE_DEBUG` &ndash; 偵錯層級設定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-292">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="e1e86-293">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="e1e86-293">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="e1e86-294">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="e1e86-294">The size of the log isn't limited.</span></span> <span data-ttu-id="e1e86-295">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="e1e86-295">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="e1e86-296">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-296">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="e1e86-297">修改堆疊大小</span><span class="sxs-lookup"><span data-stu-id="e1e86-297">Modify the stack size</span></span>

<span data-ttu-id="e1e86-298">*僅適用于使用同進程裝載模型時。*</span><span class="sxs-lookup"><span data-stu-id="e1e86-298">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="e1e86-299">使用 web.config 中的 `stackSize` 設定（以位元組為單位）來設定受控堆疊*大小。* 預設大小為 `1048576` 個位元組（1 MB）。</span><span class="sxs-lookup"><span data-stu-id="e1e86-299">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="e1e86-300">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="e1e86-300">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="e1e86-301">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="e1e86-301">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="e1e86-302">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-302">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="e1e86-303">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="e1e86-303">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="e1e86-304">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="e1e86-304">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="e1e86-305">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-305">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="e1e86-306">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-306">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="e1e86-307">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="e1e86-307">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="e1e86-308">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-308">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="e1e86-309">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="e1e86-309">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="e1e86-310">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-310">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="e1e86-311">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="e1e86-311">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="e1e86-312">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="e1e86-312">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="e1e86-313">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="e1e86-313">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="e1e86-314">在與 IIS 安裝相同的電腦上使用 IIS 共用設定時，請執行 ASP.NET Core 裝載套件組合安裝程式並將 `OPT_NO_SHARED_CONFIG_CHECK` 參數設為 `1`：</span><span class="sxs-lookup"><span data-stu-id="e1e86-314">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="e1e86-315">若共用設定的路徑位於與 IIS 安裝不同的電腦上，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="e1e86-315">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="e1e86-316">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="e1e86-316">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="e1e86-317">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="e1e86-317">Run the installer.</span></span>
1. <span data-ttu-id="e1e86-318">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="e1e86-318">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="e1e86-319">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="e1e86-319">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="e1e86-320">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="e1e86-320">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="e1e86-321">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="e1e86-321">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="e1e86-322">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="e1e86-322">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="e1e86-323">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="e1e86-323">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="e1e86-324">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="e1e86-324">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="e1e86-325">選取 [**詳細資料**] 索引標籤。檔案**版本**和**產品版本**代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="e1e86-325">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="e1e86-326">模組的裝載套件組合安裝程式記錄檔位於*C： \\Users \\% UserName% \\AppData \\Local \\Temp*。此檔案的名稱為*dd_DotNetCoreWinSvrHosting__ \<timestamp > _000_AspNetCoreModule_x64*。</span><span class="sxs-lookup"><span data-stu-id="e1e86-326">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="e1e86-327">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="e1e86-327">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="e1e86-328">Module</span><span class="sxs-lookup"><span data-stu-id="e1e86-328">Module</span></span>

<span data-ttu-id="e1e86-329">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="e1e86-329">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="e1e86-330">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-330">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="e1e86-331">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-331">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="e1e86-332">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-332">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="e1e86-333">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-333">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="e1e86-334">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="e1e86-334">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="e1e86-335">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-335">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="e1e86-336">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-336">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="e1e86-337">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-337">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="e1e86-338">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-338">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="e1e86-339">結構描述</span><span class="sxs-lookup"><span data-stu-id="e1e86-339">Schema</span></span>

<span data-ttu-id="e1e86-340">**IIS**</span><span class="sxs-lookup"><span data-stu-id="e1e86-340">**IIS**</span></span>

* <span data-ttu-id="e1e86-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="e1e86-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="e1e86-342">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="e1e86-342">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="e1e86-343">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="e1e86-343">**IIS Express**</span></span>

* <span data-ttu-id="e1e86-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="e1e86-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="e1e86-345">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="e1e86-345">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="e1e86-346">Configuration</span><span class="sxs-lookup"><span data-stu-id="e1e86-346">Configuration</span></span>

<span data-ttu-id="e1e86-347">**IIS**</span><span class="sxs-lookup"><span data-stu-id="e1e86-347">**IIS**</span></span>

* <span data-ttu-id="e1e86-348">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="e1e86-348">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="e1e86-349">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="e1e86-349">**IIS Express**</span></span>

* <span data-ttu-id="e1e86-350">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="e1e86-350">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="e1e86-351">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="e1e86-351">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="e1e86-352">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="e1e86-352">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="e1e86-353">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="e1e86-353">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="e1e86-354">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="e1e86-354">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="e1e86-355">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="e1e86-355">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="e1e86-356">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="e1e86-356">Supported Windows versions:</span></span>

* <span data-ttu-id="e1e86-357">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e1e86-357">Windows 7 or later</span></span>
* <span data-ttu-id="e1e86-358">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e1e86-358">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="e1e86-359">同處理序裝載時，模組會使用 IIS 的同處理序伺服程式實作，稱為 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-359">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="e1e86-360">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="e1e86-360">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="e1e86-361">此模組[無法與 HTTP.sys 搭配運作。](xref:fundamentals/servers/httpsys)</span><span class="sxs-lookup"><span data-stu-id="e1e86-361">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="e1e86-362">裝載模型</span><span class="sxs-lookup"><span data-stu-id="e1e86-362">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="e1e86-363">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="e1e86-363">In-process hosting model</span></span>

<span data-ttu-id="e1e86-364">若要設定同處理序裝載的應用程式，請將 `<AspNetCoreHostingModel>` 屬性新增至應用程式的專案檔，其值為 `InProcess` (跨處理序裝載是使用 `OutOfProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="e1e86-364">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="e1e86-365">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="e1e86-365">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="e1e86-366">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-366">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="e1e86-367">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="e1e86-367">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="e1e86-368">使用 IIS HTTP 伺服器 (`IISHttpServer`) 而不是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器。</span><span class="sxs-lookup"><span data-stu-id="e1e86-368">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="e1e86-369">針對同處理序，[CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 來：</span><span class="sxs-lookup"><span data-stu-id="e1e86-369">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="e1e86-370">註冊 `IISHttpServer`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-370">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="e1e86-371">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-371">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="e1e86-372">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="e1e86-372">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="e1e86-373">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="e1e86-373">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="e1e86-374">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="e1e86-374">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="e1e86-375">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="e1e86-375">Use one app pool per app.</span></span>

* <span data-ttu-id="e1e86-376">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="e1e86-376">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="e1e86-377">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="e1e86-377">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="e1e86-378">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="e1e86-378">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="e1e86-379">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="e1e86-379">Client disconnects are detected.</span></span> <span data-ttu-id="e1e86-380">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="e1e86-380">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="e1e86-381">在 ASP.NET Core 2.2.1 或更早版本中，<xref:System.IO.Directory.GetCurrentDirectory*> 會傳回 IIS 所啟動之處理序的背景工作目錄，而非應用程式的目錄 (例如 *w3wp.exe* 為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-381">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="e1e86-382">如需設定應用程式目前所在目錄的範例程式碼，請參閱 [CurrentDirectoryHelpers 類別](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-382">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="e1e86-383">呼叫 `SetCurrentDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="e1e86-383">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="e1e86-384">後續呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 會提供應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-384">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="e1e86-385">裝載同處理序時，不會內部呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 來將使用者初始化。</span><span class="sxs-lookup"><span data-stu-id="e1e86-385">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="e1e86-386">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="e1e86-386">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="e1e86-387">使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告時，請呼叫 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 來新增入驗證服務：</span><span class="sxs-lookup"><span data-stu-id="e1e86-387">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="e1e86-388">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="e1e86-388">Out-of-process hosting model</span></span>

<span data-ttu-id="e1e86-389">若要設定跨處理序裝載的應用程式，請在專案檔中使用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="e1e86-389">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="e1e86-390">請勿指定 `<AspNetCoreHostingModel>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-390">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="e1e86-391">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-391">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="e1e86-392">將 `<AspNetCoreHostingModel>` 屬性的值設定為 `OutOfProcess` (同處理序裝載是使用 `InProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="e1e86-392">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="e1e86-393">使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器而不是 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-393">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="e1e86-394">針對跨處理序，[CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 來：</span><span class="sxs-lookup"><span data-stu-id="e1e86-394">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="e1e86-395">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-395">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="e1e86-396">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="e1e86-396">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="e1e86-397">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="e1e86-397">Hosting model changes</span></span>

<span data-ttu-id="e1e86-398">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-398">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="e1e86-399">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="e1e86-399">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="e1e86-400">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-400">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="e1e86-401">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="e1e86-401">Process name</span></span>

<span data-ttu-id="e1e86-402">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-402">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="e1e86-403">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="e1e86-403">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="e1e86-404">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="e1e86-404">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="e1e86-405">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="e1e86-405">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="e1e86-406">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-406">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="e1e86-407">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="e1e86-407">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="e1e86-408">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="e1e86-408">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="e1e86-409">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="e1e86-409">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="e1e86-410">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-410">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="e1e86-411">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="e1e86-411">Configuration with web.config</span></span>

<span data-ttu-id="e1e86-412">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-412">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="e1e86-413">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="e1e86-413">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="e1e86-414">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="e1e86-414">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="e1e86-415">將 <xref:System.Configuration.SectionInformation.InheritInChildApplications*> 屬性設定為 `false`，以表示在 [\<位置>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 內指定的設定，不是由位在應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="e1e86-415">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="e1e86-416">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-416">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="e1e86-417">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="e1e86-417">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="e1e86-418">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="e1e86-418">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="e1e86-419">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="e1e86-419">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="e1e86-420">屬性</span><span class="sxs-lookup"><span data-stu-id="e1e86-420">Attribute</span></span> | <span data-ttu-id="e1e86-421">描述</span><span class="sxs-lookup"><span data-stu-id="e1e86-421">Description</span></span> | <span data-ttu-id="e1e86-422">Default</span><span class="sxs-lookup"><span data-stu-id="e1e86-422">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="e1e86-423">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-423">Optional string attribute.</span></span></p><p><span data-ttu-id="e1e86-424">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-424">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="e1e86-425">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-425">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="e1e86-426">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-426">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="e1e86-427">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-427">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="e1e86-428">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-428">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="e1e86-429">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="e1e86-429">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="e1e86-430">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-430">Optional string attribute.</span></span></p><p><span data-ttu-id="e1e86-431">將裝載模型指定為同處理序 (`InProcess`) 或跨處理序 (`OutOfProcess`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-431">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="e1e86-432">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-432">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-433">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="e1e86-433">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="e1e86-434">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-434">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="e1e86-435">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-435">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="e1e86-436">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="e1e86-436">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="e1e86-437">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="e1e86-437">Default: `1`</span></span><br><span data-ttu-id="e1e86-438">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="e1e86-438">Min: `1`</span></span><br><span data-ttu-id="e1e86-439">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="e1e86-439">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="e1e86-440">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-440">Required string attribute.</span></span></p><p><span data-ttu-id="e1e86-441">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-441">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="e1e86-442">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-442">Relative paths are supported.</span></span> <span data-ttu-id="e1e86-443">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-443">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="e1e86-444">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-444">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-445">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-445">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="e1e86-446">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-446">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="e1e86-447">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="e1e86-447">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="e1e86-448">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="e1e86-448">Default: `10`</span></span><br><span data-ttu-id="e1e86-449">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="e1e86-449">Min: `0`</span></span><br><span data-ttu-id="e1e86-450">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="e1e86-450">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="e1e86-451">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-451">Optional timespan attribute.</span></span></p><p><span data-ttu-id="e1e86-452">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="e1e86-452">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="e1e86-453">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-453">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="e1e86-454">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="e1e86-454">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="e1e86-455">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-455">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="e1e86-456">字串之分鐘和秒數的有效值介於 0-59。</span><span class="sxs-lookup"><span data-stu-id="e1e86-456">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="e1e86-457">在分鐘或秒數的值中使用 **60** 將會導致「500 - 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="e1e86-457">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="e1e86-458">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="e1e86-458">Default: `00:02:00`</span></span><br><span data-ttu-id="e1e86-459">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="e1e86-459">Min: `00:00:00`</span></span><br><span data-ttu-id="e1e86-460">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="e1e86-460">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="e1e86-461">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-461">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-462">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-462">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="e1e86-463">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="e1e86-463">Default: `10`</span></span><br><span data-ttu-id="e1e86-464">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="e1e86-464">Min: `0`</span></span><br><span data-ttu-id="e1e86-465">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="e1e86-465">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="e1e86-466">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-466">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-467">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-467">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="e1e86-468">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-468">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="e1e86-469">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-469">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="e1e86-470">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="e1e86-470">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="e1e86-471">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="e1e86-471">Default: `120`</span></span><br><span data-ttu-id="e1e86-472">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="e1e86-472">Min: `0`</span></span><br><span data-ttu-id="e1e86-473">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="e1e86-473">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="e1e86-474">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-474">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="e1e86-475">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="e1e86-475">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="e1e86-476">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-476">Optional string attribute.</span></span></p><p><span data-ttu-id="e1e86-477">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-477">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="e1e86-478">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-478">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="e1e86-479">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-479">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="e1e86-480">建立記錄檔後，模組會建立路徑中提供的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="e1e86-480">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="e1e86-481">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 ( *.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="e1e86-481">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="e1e86-482">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e1e86-482">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="e1e86-483">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="e1e86-483">Setting environment variables</span></span>

<span data-ttu-id="e1e86-484">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-484">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="e1e86-485">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-485">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="e1e86-486">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-486">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="e1e86-487">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-487">The following example sets two environment variables.</span></span> <span data-ttu-id="e1e86-488">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-488">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="e1e86-489">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="e1e86-489">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="e1e86-490">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-490">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> <span data-ttu-id="e1e86-491">在 *web.config* 中直接設定環境的替代方式是在發行設定檔 ( *.pubxml*) 或專案檔中包括 `<EnvironmentName>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-491">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="e1e86-492">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="e1e86-492">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="e1e86-493">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-493">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="e1e86-494">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="e1e86-494">app_offline.htm</span></span>

<span data-ttu-id="e1e86-495">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-495">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="e1e86-496">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-496">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="e1e86-497">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-497">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="e1e86-498">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e1e86-498">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="e1e86-499">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="e1e86-499">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="e1e86-500">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="e1e86-500">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="e1e86-501">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="e1e86-501">Start-up error page</span></span>

<span data-ttu-id="e1e86-502">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-502">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="e1e86-503">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-503">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="e1e86-504">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-504">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="e1e86-505">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-505">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="e1e86-506">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-506">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="e1e86-507">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-507">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="e1e86-508">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="e1e86-508">Log creation and redirection</span></span>

<span data-ttu-id="e1e86-509">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="e1e86-509">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="e1e86-510">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="e1e86-510">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="e1e86-511">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-511">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="e1e86-512">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-512">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="e1e86-513">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="e1e86-513">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="e1e86-514">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-514">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="e1e86-515">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-515">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="e1e86-516">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="e1e86-516">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="e1e86-517">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-517">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="e1e86-518">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="e1e86-518">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="e1e86-519">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 ( *.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="e1e86-519">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="e1e86-520">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="e1e86-520">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="e1e86-521">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-521">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="e1e86-522">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-522">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="e1e86-523">下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-523">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="e1e86-524">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-524">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="e1e86-525">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="e1e86-525">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="e1e86-526">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="e1e86-526">Enhanced diagnostic logs</span></span>

<span data-ttu-id="e1e86-527">ASP.NET Core 模組是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-527">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="e1e86-528">將 `<handlerSettings>` 專案加入*web.config*中的 `<aspNetCore>` 元素。將 `debugLevel` 設定為 `TRACE` 會對診斷資訊公開更高的精確度：</span><span class="sxs-lookup"><span data-stu-id="e1e86-528">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="e1e86-529">模組不會在提供給 `<handlerSetting>` 值的路徑 (在上述範例中為 *logs*) 中自動建立資料夾，而且這些資料夾應該已存在於部署中。</span><span class="sxs-lookup"><span data-stu-id="e1e86-529">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="e1e86-530">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-530">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="e1e86-531">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="e1e86-531">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="e1e86-532">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="e1e86-532">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="e1e86-533">ERROR</span><span class="sxs-lookup"><span data-stu-id="e1e86-533">ERROR</span></span>
* <span data-ttu-id="e1e86-534">WARNING</span><span class="sxs-lookup"><span data-stu-id="e1e86-534">WARNING</span></span>
* <span data-ttu-id="e1e86-535">INFO</span><span class="sxs-lookup"><span data-stu-id="e1e86-535">INFO</span></span>
* <span data-ttu-id="e1e86-536">TRACE</span><span class="sxs-lookup"><span data-stu-id="e1e86-536">TRACE</span></span>

<span data-ttu-id="e1e86-537">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="e1e86-537">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="e1e86-538">主控台</span><span class="sxs-lookup"><span data-stu-id="e1e86-538">CONSOLE</span></span>
* <span data-ttu-id="e1e86-539">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="e1e86-539">EVENTLOG</span></span>
* <span data-ttu-id="e1e86-540">檔案</span><span class="sxs-lookup"><span data-stu-id="e1e86-540">FILE</span></span>

<span data-ttu-id="e1e86-541">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="e1e86-541">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="e1e86-542">偵錯記錄檔的 `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; 路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-542">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="e1e86-543">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="e1e86-543">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="e1e86-544">`ASPNETCORE_MODULE_DEBUG` &ndash; 偵錯層級設定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-544">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="e1e86-545">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="e1e86-545">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="e1e86-546">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="e1e86-546">The size of the log isn't limited.</span></span> <span data-ttu-id="e1e86-547">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="e1e86-547">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="e1e86-548">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-548">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="e1e86-549">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="e1e86-549">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="e1e86-550">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="e1e86-550">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="e1e86-551">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-551">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="e1e86-552">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="e1e86-552">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="e1e86-553">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="e1e86-553">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="e1e86-554">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-554">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="e1e86-555">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-555">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="e1e86-556">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="e1e86-556">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="e1e86-557">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-557">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="e1e86-558">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="e1e86-558">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="e1e86-559">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-559">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="e1e86-560">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="e1e86-560">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="e1e86-561">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="e1e86-561">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="e1e86-562">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="e1e86-562">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="e1e86-563">在與 IIS 安裝相同的電腦上使用 IIS 共用設定時，請執行 ASP.NET Core 裝載套件組合安裝程式並將 `OPT_NO_SHARED_CONFIG_CHECK` 參數設為 `1`：</span><span class="sxs-lookup"><span data-stu-id="e1e86-563">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="e1e86-564">若共用設定的路徑位於與 IIS 安裝不同的電腦上，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="e1e86-564">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="e1e86-565">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="e1e86-565">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="e1e86-566">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="e1e86-566">Run the installer.</span></span>
1. <span data-ttu-id="e1e86-567">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="e1e86-567">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="e1e86-568">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="e1e86-568">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="e1e86-569">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="e1e86-569">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="e1e86-570">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="e1e86-570">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="e1e86-571">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="e1e86-571">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="e1e86-572">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="e1e86-572">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="e1e86-573">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="e1e86-573">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="e1e86-574">選取 [**詳細資料**] 索引標籤。檔案**版本**和**產品版本**代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="e1e86-574">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="e1e86-575">模組的裝載套件組合安裝程式記錄檔位於*C： \\Users \\% UserName% \\AppData \\Local \\Temp*。此檔案的名稱為*dd_DotNetCoreWinSvrHosting__ \<timestamp > _000_AspNetCoreModule_x64*。</span><span class="sxs-lookup"><span data-stu-id="e1e86-575">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="e1e86-576">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="e1e86-576">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="e1e86-577">Module</span><span class="sxs-lookup"><span data-stu-id="e1e86-577">Module</span></span>

<span data-ttu-id="e1e86-578">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="e1e86-578">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="e1e86-579">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-579">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="e1e86-580">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-580">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="e1e86-581">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-581">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="e1e86-582">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-582">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="e1e86-583">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="e1e86-583">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="e1e86-584">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-584">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="e1e86-585">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-585">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="e1e86-586">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-586">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="e1e86-587">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-587">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="e1e86-588">結構描述</span><span class="sxs-lookup"><span data-stu-id="e1e86-588">Schema</span></span>

<span data-ttu-id="e1e86-589">**IIS**</span><span class="sxs-lookup"><span data-stu-id="e1e86-589">**IIS**</span></span>

* <span data-ttu-id="e1e86-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="e1e86-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="e1e86-591">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="e1e86-591">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="e1e86-592">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="e1e86-592">**IIS Express**</span></span>

* <span data-ttu-id="e1e86-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="e1e86-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="e1e86-594">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="e1e86-594">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="e1e86-595">Configuration</span><span class="sxs-lookup"><span data-stu-id="e1e86-595">Configuration</span></span>

<span data-ttu-id="e1e86-596">**IIS**</span><span class="sxs-lookup"><span data-stu-id="e1e86-596">**IIS**</span></span>

* <span data-ttu-id="e1e86-597">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="e1e86-597">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="e1e86-598">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="e1e86-598">**IIS Express**</span></span>

* <span data-ttu-id="e1e86-599">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="e1e86-599">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="e1e86-600">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="e1e86-600">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="e1e86-601">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="e1e86-601">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e1e86-602">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線，將 Web 要求重新轉送到後端的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e1e86-602">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="e1e86-603">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="e1e86-603">Supported Windows versions:</span></span>

* <span data-ttu-id="e1e86-604">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e1e86-604">Windows 7 or later</span></span>
* <span data-ttu-id="e1e86-605">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e1e86-605">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="e1e86-606">該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="e1e86-606">The module only works with Kestrel.</span></span> <span data-ttu-id="e1e86-607">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) 不相容。</span><span class="sxs-lookup"><span data-stu-id="e1e86-607">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="e1e86-608">因為處理序中執行的 ASP.NET Core 應用程式會與 IIS 背景工作處理序分開，所以此模組也會執行處理序管理。</span><span class="sxs-lookup"><span data-stu-id="e1e86-608">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="e1e86-609">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="e1e86-609">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="e1e86-610">此行為基本上與在 IIS 中執行同處理序，並由 [Windows 處理器啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的 ASP.NET 4.x 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="e1e86-610">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="e1e86-611">下圖說明 IIS、ASP.NET Core 模組和應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="e1e86-611">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core 模組](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="e1e86-613">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="e1e86-613">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="e1e86-614">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-614">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="e1e86-615">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="e1e86-615">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="e1e86-616">此模組在啟動時透過環境變數指定連接埠，而 [IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-616">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="e1e86-617">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="e1e86-617">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="e1e86-618">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="e1e86-618">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="e1e86-619">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="e1e86-619">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="e1e86-620">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="e1e86-620">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="e1e86-621">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="e1e86-621">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="e1e86-622">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e1e86-622">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="e1e86-623">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="e1e86-623">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="e1e86-624">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="e1e86-624">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="e1e86-625">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="e1e86-625">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="e1e86-626">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-626">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="e1e86-627">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="e1e86-627">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="e1e86-628">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="e1e86-628">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="e1e86-629">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="e1e86-629">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="e1e86-630">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-630">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="e1e86-631">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="e1e86-631">Configuration with web.config</span></span>

<span data-ttu-id="e1e86-632">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-632">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="e1e86-633">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="e1e86-633">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="e1e86-634">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="e1e86-634">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="e1e86-635">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-635">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="e1e86-636">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="e1e86-636">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="e1e86-637">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="e1e86-637">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="e1e86-638">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="e1e86-638">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="e1e86-639">屬性</span><span class="sxs-lookup"><span data-stu-id="e1e86-639">Attribute</span></span> | <span data-ttu-id="e1e86-640">描述</span><span class="sxs-lookup"><span data-stu-id="e1e86-640">Description</span></span> | <span data-ttu-id="e1e86-641">Default</span><span class="sxs-lookup"><span data-stu-id="e1e86-641">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="e1e86-642">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-642">Optional string attribute.</span></span></p><p><span data-ttu-id="e1e86-643">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-643">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="e1e86-644">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-644">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="e1e86-645">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-645">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="e1e86-646">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-646">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="e1e86-647">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-647">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="e1e86-648">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="e1e86-648">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="e1e86-649">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-649">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-650">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="e1e86-650">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="e1e86-651">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-651">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="e1e86-652">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="e1e86-652">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="e1e86-653">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="e1e86-653">Default: `1`</span></span><br><span data-ttu-id="e1e86-654">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="e1e86-654">Min: `1`</span></span><br><span data-ttu-id="e1e86-655">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="e1e86-655">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="e1e86-656">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-656">Required string attribute.</span></span></p><p><span data-ttu-id="e1e86-657">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-657">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="e1e86-658">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-658">Relative paths are supported.</span></span> <span data-ttu-id="e1e86-659">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-659">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="e1e86-660">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-660">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-661">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-661">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="e1e86-662">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-662">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="e1e86-663">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="e1e86-663">Default: `10`</span></span><br><span data-ttu-id="e1e86-664">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="e1e86-664">Min: `0`</span></span><br><span data-ttu-id="e1e86-665">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="e1e86-665">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="e1e86-666">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-666">Optional timespan attribute.</span></span></p><p><span data-ttu-id="e1e86-667">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="e1e86-667">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="e1e86-668">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-668">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="e1e86-669">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="e1e86-669">Default: `00:02:00`</span></span><br><span data-ttu-id="e1e86-670">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="e1e86-670">Min: `00:00:00`</span></span><br><span data-ttu-id="e1e86-671">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="e1e86-671">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="e1e86-672">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-672">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-673">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-673">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="e1e86-674">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="e1e86-674">Default: `10`</span></span><br><span data-ttu-id="e1e86-675">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="e1e86-675">Min: `0`</span></span><br><span data-ttu-id="e1e86-676">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="e1e86-676">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="e1e86-677">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-677">Optional integer attribute.</span></span></p><p><span data-ttu-id="e1e86-678">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-678">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="e1e86-679">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-679">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="e1e86-680">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-680">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="e1e86-681">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="e1e86-681">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="e1e86-682">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="e1e86-682">Default: `120`</span></span><br><span data-ttu-id="e1e86-683">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="e1e86-683">Min: `0`</span></span><br><span data-ttu-id="e1e86-684">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="e1e86-684">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="e1e86-685">選擇性的布林值屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-685">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="e1e86-686">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="e1e86-686">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="e1e86-687">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-687">Optional string attribute.</span></span></p><p><span data-ttu-id="e1e86-688">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-688">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="e1e86-689">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-689">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="e1e86-690">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-690">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="e1e86-691">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-691">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="e1e86-692">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 ( *.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="e1e86-692">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="e1e86-693">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e1e86-693">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="e1e86-694">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="e1e86-694">Setting environment variables</span></span>

<span data-ttu-id="e1e86-695">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-695">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="e1e86-696">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-696">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="e1e86-697">此節中所設定的環境變數與使用相同名稱設定的系統環境變數相衝突。</span><span class="sxs-lookup"><span data-stu-id="e1e86-697">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="e1e86-698">若同時在 *web.config* 檔案與 Windows 中的系統層級設定環境變數，來自 *web.config* 檔案的值會成為附加到系統環境變數值 (例如，`ASPNETCORE_ENVIRONMENT: Development;Development`)，這會造成應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="e1e86-698">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="e1e86-699">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="e1e86-699">The following example sets two environment variables.</span></span> <span data-ttu-id="e1e86-700">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-700">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="e1e86-701">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="e1e86-701">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="e1e86-702">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="e1e86-702">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="e1e86-703">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="e1e86-703">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="e1e86-704">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="e1e86-704">app_offline.htm</span></span>

<span data-ttu-id="e1e86-705">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-705">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="e1e86-706">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="e1e86-706">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="e1e86-707">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-707">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="e1e86-708">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e1e86-708">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="e1e86-709">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="e1e86-709">Start-up error page</span></span>

<span data-ttu-id="e1e86-710">如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="e1e86-710">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="e1e86-711">若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e1e86-711">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="e1e86-712">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-712">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="e1e86-714">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="e1e86-714">Log creation and redirection</span></span>

<span data-ttu-id="e1e86-715">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="e1e86-715">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="e1e86-716">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="e1e86-716">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="e1e86-717">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-717">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="e1e86-718">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-718">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="e1e86-719">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="e1e86-719">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="e1e86-720">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e1e86-720">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="e1e86-721">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-721">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="e1e86-722">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="e1e86-722">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="e1e86-723">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-723">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="e1e86-724">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="e1e86-724">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="e1e86-725">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 ( *.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="e1e86-725">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="e1e86-726">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="e1e86-726">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="e1e86-727">下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-727">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="e1e86-728">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="e1e86-728">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="e1e86-729">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="e1e86-729">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="e1e86-730">模組不會在提供給 `<handlerSetting>` 值的路徑 (在上述範例中為 *logs*) 中自動建立資料夾，而且這些資料夾應該已存在於部署中。</span><span class="sxs-lookup"><span data-stu-id="e1e86-730">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="e1e86-731">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-731">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="e1e86-732">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-732">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="e1e86-733">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="e1e86-733">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="e1e86-734">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="e1e86-734">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="e1e86-735">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="e1e86-735">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="e1e86-736">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="e1e86-736">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="e1e86-737">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-737">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="e1e86-738">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="e1e86-738">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="e1e86-739">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="e1e86-739">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="e1e86-740">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-740">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="e1e86-741">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="e1e86-741">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="e1e86-742">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="e1e86-742">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="e1e86-743">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="e1e86-743">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="e1e86-744">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="e1e86-744">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="e1e86-745">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="e1e86-745">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="e1e86-746">使用「IIS 共用設定」時，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="e1e86-746">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="e1e86-747">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="e1e86-747">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="e1e86-748">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="e1e86-748">Run the installer.</span></span>
1. <span data-ttu-id="e1e86-749">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="e1e86-749">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="e1e86-750">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="e1e86-750">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="e1e86-751">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="e1e86-751">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="e1e86-752">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="e1e86-752">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="e1e86-753">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="e1e86-753">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="e1e86-754">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="e1e86-754">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="e1e86-755">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="e1e86-755">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="e1e86-756">選取 [**詳細資料**] 索引標籤。檔案**版本**和**產品版本**代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="e1e86-756">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="e1e86-757">模組的裝載套件組合安裝程式記錄檔位於*C： \\Users \\% UserName% \\AppData \\Local \\Temp*。此檔案的名稱為*dd_DotNetCoreWinSvrHosting__ \<timestamp > _000_AspNetCoreModule_x64*。</span><span class="sxs-lookup"><span data-stu-id="e1e86-757">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="e1e86-758">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="e1e86-758">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="e1e86-759">Module</span><span class="sxs-lookup"><span data-stu-id="e1e86-759">Module</span></span>

<span data-ttu-id="e1e86-760">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="e1e86-760">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="e1e86-761">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-761">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="e1e86-762">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-762">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="e1e86-763">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="e1e86-763">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="e1e86-764">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-764">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="e1e86-765">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="e1e86-765">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="e1e86-766">結構描述</span><span class="sxs-lookup"><span data-stu-id="e1e86-766">Schema</span></span>

<span data-ttu-id="e1e86-767">**IIS**</span><span class="sxs-lookup"><span data-stu-id="e1e86-767">**IIS**</span></span>

* <span data-ttu-id="e1e86-768">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="e1e86-768">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="e1e86-769">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="e1e86-769">**IIS Express**</span></span>

* <span data-ttu-id="e1e86-770">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="e1e86-770">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="e1e86-771">Configuration</span><span class="sxs-lookup"><span data-stu-id="e1e86-771">Configuration</span></span>

<span data-ttu-id="e1e86-772">**IIS**</span><span class="sxs-lookup"><span data-stu-id="e1e86-772">**IIS**</span></span>

* <span data-ttu-id="e1e86-773">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="e1e86-773">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="e1e86-774">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="e1e86-774">**IIS Express**</span></span>

* <span data-ttu-id="e1e86-775">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="e1e86-775">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="e1e86-776">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="e1e86-776">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="e1e86-777">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="e1e86-777">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e1e86-778">其他資源</span><span class="sxs-lookup"><span data-stu-id="e1e86-778">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="e1e86-779">ASP.NET Core 模組 GitHub 存放庫 (參考來源)</span><span class="sxs-lookup"><span data-stu-id="e1e86-779">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
