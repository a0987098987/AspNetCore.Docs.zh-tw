---
title: ASP.NET Core 模組
author: guardrex
description: 了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c1c34f368cb3f7767bf0f229ff70c5ab53c6005f
ms.sourcegitcommit: fcdf9aaa6c45c1a926bd870ed8f893bdb4935152
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165318"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="cb852-103">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="cb852-103">ASP.NET Core Module</span></span>

<span data-ttu-id="cb852-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti)、[Justin Kotalik](https://github.com/jkotalik) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cb852-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cb852-105">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="cb852-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="cb852-106">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="cb852-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="cb852-107">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="cb852-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="cb852-108">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="cb852-108">Supported Windows versions:</span></span>

* <span data-ttu-id="cb852-109">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="cb852-109">Windows 7 or later</span></span>
* <span data-ttu-id="cb852-110">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="cb852-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="cb852-111">同處理序裝載時，模組會使用 IIS 的同處理序伺服程式實作，稱為 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="cb852-112">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="cb852-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="cb852-113">此模組[無法與 HTTP.sys 搭配運作。](xref:fundamentals/servers/httpsys)</span><span class="sxs-lookup"><span data-stu-id="cb852-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="cb852-114">裝載模型</span><span class="sxs-lookup"><span data-stu-id="cb852-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="cb852-115">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="cb852-115">In-process hosting model</span></span>

<span data-ttu-id="cb852-116">ASP.NET Core 應用程式預設為同進程裝載模型。</span><span class="sxs-lookup"><span data-stu-id="cb852-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="cb852-117">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="cb852-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="cb852-118">使用 IIS HTTP 伺服器 (`IISHttpServer`) 而不是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器。</span><span class="sxs-lookup"><span data-stu-id="cb852-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="cb852-119">針對同處理序，[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 來：</span><span class="sxs-lookup"><span data-stu-id="cb852-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="cb852-120">註冊 `IISHttpServer`。</span><span class="sxs-lookup"><span data-stu-id="cb852-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="cb852-121">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="cb852-122">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="cb852-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="cb852-123">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="cb852-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="cb852-124">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="cb852-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="cb852-125">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="cb852-125">Use one app pool per app.</span></span>

* <span data-ttu-id="cb852-126">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="cb852-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="cb852-127">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="cb852-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="cb852-128">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="cb852-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="cb852-129">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="cb852-129">Client disconnects are detected.</span></span> <span data-ttu-id="cb852-130">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="cb852-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="cb852-131">在 ASP.NET Core 2.2.1 或更早版本中，<xref:System.IO.Directory.GetCurrentDirectory*> 會傳回 IIS 所啟動之處理序的背景工作目錄，而非應用程式的目錄 (例如 *w3wp.exe* 為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="cb852-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="cb852-132">如需設定應用程式目前所在目錄的範例程式碼，請參閱 [CurrentDirectoryHelpers 類別](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs)。</span><span class="sxs-lookup"><span data-stu-id="cb852-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="cb852-133">呼叫 `SetCurrentDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="cb852-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="cb852-134">後續呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 會提供應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="cb852-135">裝載同處理序時，不會內部呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 來將使用者初始化。</span><span class="sxs-lookup"><span data-stu-id="cb852-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="cb852-136">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="cb852-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="cb852-137">使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告時，請呼叫 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 來新增入驗證服務：</span><span class="sxs-lookup"><span data-stu-id="cb852-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="cb852-138">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="cb852-138">Out-of-process hosting model</span></span>

<span data-ttu-id="cb852-139">若要設定跨進程裝載的應用程式，請在專案檔（ *.csproj*）中將 [`<AspNetCoreHostingModel>`] 屬性的值設為 `OutOfProcess`：</span><span class="sxs-lookup"><span data-stu-id="cb852-139">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="cb852-140">同進程裝載是使用 `InProcess` 來設定，這是預設值。</span><span class="sxs-lookup"><span data-stu-id="cb852-140">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="cb852-141">使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器而不是 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-141">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="cb852-142">針對跨處理序，[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 來：</span><span class="sxs-lookup"><span data-stu-id="cb852-142">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="cb852-143">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-143">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="cb852-144">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="cb852-144">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="cb852-145">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="cb852-145">Hosting model changes</span></span>

<span data-ttu-id="cb852-146">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-146">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="cb852-147">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="cb852-147">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="cb852-148">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-148">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="cb852-149">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="cb852-149">Process name</span></span>

<span data-ttu-id="cb852-150">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="cb852-150">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="cb852-151">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="cb852-151">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="cb852-152">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="cb852-152">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="cb852-153">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="cb852-153">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="cb852-154">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-154">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="cb852-155">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="cb852-155">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="cb852-156">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="cb852-156">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="cb852-157">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="cb852-157">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="cb852-158">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="cb852-158">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="cb852-159">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="cb852-159">Configuration with web.config</span></span>

<span data-ttu-id="cb852-160">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="cb852-160">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="cb852-161">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="cb852-161">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="cb852-162">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="cb852-162">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="cb852-163">將 <xref:System.Configuration.SectionInformation.InheritInChildApplications*> 屬性設定為 `false`，以表示在 [\<位置>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 內指定的設定，不是由位在應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="cb852-163">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="cb852-164">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="cb852-164">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="cb852-165">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="cb852-165">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="cb852-166">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="cb852-166">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="cb852-167">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="cb852-167">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="cb852-168">屬性</span><span class="sxs-lookup"><span data-stu-id="cb852-168">Attribute</span></span> | <span data-ttu-id="cb852-169">描述</span><span class="sxs-lookup"><span data-stu-id="cb852-169">Description</span></span> | <span data-ttu-id="cb852-170">預設</span><span class="sxs-lookup"><span data-stu-id="cb852-170">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="cb852-171">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-171">Optional string attribute.</span></span></p><p><span data-ttu-id="cb852-172">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="cb852-172">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="cb852-173">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-173">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cb852-174">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-174">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="cb852-175">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-175">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cb852-176">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-176">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="cb852-177">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="cb852-177">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="cb852-178">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-178">Optional string attribute.</span></span></p><p><span data-ttu-id="cb852-179">將裝載模型指定為同處理序 (`InProcess`) 或跨處理序 (`OutOfProcess`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-179">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `InProcess` |
| `processesPerApplication` | <p><span data-ttu-id="cb852-180">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-180">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-181">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="cb852-181">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="cb852-182">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="cb852-182">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="cb852-183">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="cb852-183">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="cb852-184">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="cb852-184">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="cb852-185">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="cb852-185">Default: `1`</span></span><br><span data-ttu-id="cb852-186">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="cb852-186">Min: `1`</span></span><br><span data-ttu-id="cb852-187">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="cb852-187">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="cb852-188">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-188">Required string attribute.</span></span></p><p><span data-ttu-id="cb852-189">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-189">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="cb852-190">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-190">Relative paths are supported.</span></span> <span data-ttu-id="cb852-191">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-191">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="cb852-192">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-192">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-193">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="cb852-193">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="cb852-194">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-194">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="cb852-195">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="cb852-195">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="cb852-196">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="cb852-196">Default: `10`</span></span><br><span data-ttu-id="cb852-197">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="cb852-197">Min: `0`</span></span><br><span data-ttu-id="cb852-198">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="cb852-198">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="cb852-199">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-199">Optional timespan attribute.</span></span></p><p><span data-ttu-id="cb852-200">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="cb852-200">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="cb852-201">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="cb852-201">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="cb852-202">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="cb852-202">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="cb852-203">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-203">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="cb852-204">字串之分鐘和秒數的有效值介於 0-59。</span><span class="sxs-lookup"><span data-stu-id="cb852-204">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="cb852-205">在分鐘或秒數的值中使用 **60** 將會導致「500 - 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="cb852-205">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="cb852-206">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="cb852-206">Default: `00:02:00`</span></span><br><span data-ttu-id="cb852-207">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="cb852-207">Min: `00:00:00`</span></span><br><span data-ttu-id="cb852-208">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="cb852-208">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="cb852-209">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-209">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-210">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="cb852-210">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="cb852-211">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="cb852-211">Default: `10`</span></span><br><span data-ttu-id="cb852-212">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="cb852-212">Min: `0`</span></span><br><span data-ttu-id="cb852-213">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="cb852-213">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="cb852-214">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-214">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-215">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="cb852-215">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="cb852-216">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-216">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="cb852-217">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="cb852-217">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="cb852-218">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="cb852-218">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="cb852-219">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="cb852-219">Default: `120`</span></span><br><span data-ttu-id="cb852-220">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="cb852-220">Min: `0`</span></span><br><span data-ttu-id="cb852-221">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="cb852-221">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="cb852-222">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-222">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cb852-223">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="cb852-223">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="cb852-224">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-224">Optional string attribute.</span></span></p><p><span data-ttu-id="cb852-225">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-225">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="cb852-226">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-226">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="cb852-227">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-227">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="cb852-228">建立記錄檔後，模組會建立路徑中提供的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="cb852-228">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="cb852-229">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 ( *.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="cb852-229">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="cb852-230">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="cb852-230">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="cb852-231">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="cb852-231">Set environment variables</span></span>

<span data-ttu-id="cb852-232">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-232">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="cb852-233">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-233">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="cb852-234">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-234">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="cb852-235">下列範例會在*web.config 中設定*兩個環境變數。`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="cb852-235">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="cb852-236">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="cb852-236">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="cb852-237">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-237">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="cb852-238">在 *web.config* 中直接設定環境的替代方式是在發行設定檔 ( *.pubxml*) 或專案檔中包括 `<EnvironmentName>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-238">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="cb852-239">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="cb852-239">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="cb852-240">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="cb852-240">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="cb852-241">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="cb852-241">app_offline.htm</span></span>

<span data-ttu-id="cb852-242">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-242">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="cb852-243">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-243">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="cb852-244">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-244">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="cb852-245">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb852-245">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="cb852-246">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="cb852-246">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="cb852-247">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="cb852-247">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="cb852-248">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="cb852-248">Start-up error page</span></span>

<span data-ttu-id="cb852-249">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-249">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="cb852-250">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-250">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="cb852-251">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-251">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="cb852-252">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-252">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="cb852-253">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-253">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="cb852-254">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="cb852-254">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="cb852-255">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="cb852-255">Log creation and redirection</span></span>

<span data-ttu-id="cb852-256">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="cb852-256">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="cb852-257">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="cb852-257">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="cb852-258">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="cb852-258">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="cb852-259">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-259">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="cb852-260">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="cb852-260">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="cb852-261">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-261">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="cb852-262">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-262">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="cb852-263">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="cb852-263">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="cb852-264">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="cb852-264">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="cb852-265">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="cb852-265">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="cb852-266">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 ( *.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="cb852-266">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="cb852-267">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="cb852-267">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="cb852-268">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-268">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="cb852-269">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-269">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="cb852-270">*Web.config 檔案*中的下列範例 `aspNetCore` 元素會針對 Azure App Service 中裝載的應用程式，設定 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-270">The following sample `aspNetCore` element in a *web.config* file configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="cb852-271">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-271">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="cb852-272">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="cb852-272">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="cb852-273">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="cb852-273">Enhanced diagnostic logs</span></span>

<span data-ttu-id="cb852-274">ASP.NET Core 模組是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-274">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="cb852-275">將 `<handlerSettings>` 項目新增至 *web.config* 中的 `<aspNetCore>` 項目。將 `debugLevel` 設定為 `TRACE` 會公開精確性更高的診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="cb852-275">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="cb852-276">建立記錄檔後，模組會建立路徑 (在上述範例中為 *logs*) 中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="cb852-276">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="cb852-277">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="cb852-277">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="cb852-278">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="cb852-278">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="cb852-279">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="cb852-279">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="cb852-280">ERROR</span><span class="sxs-lookup"><span data-stu-id="cb852-280">ERROR</span></span>
* <span data-ttu-id="cb852-281">WARNING</span><span class="sxs-lookup"><span data-stu-id="cb852-281">WARNING</span></span>
* <span data-ttu-id="cb852-282">INFO</span><span class="sxs-lookup"><span data-stu-id="cb852-282">INFO</span></span>
* <span data-ttu-id="cb852-283">TRACE</span><span class="sxs-lookup"><span data-stu-id="cb852-283">TRACE</span></span>

<span data-ttu-id="cb852-284">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="cb852-284">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="cb852-285">主控台</span><span class="sxs-lookup"><span data-stu-id="cb852-285">CONSOLE</span></span>
* <span data-ttu-id="cb852-286">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="cb852-286">EVENTLOG</span></span>
* <span data-ttu-id="cb852-287">檔案</span><span class="sxs-lookup"><span data-stu-id="cb852-287">FILE</span></span>

<span data-ttu-id="cb852-288">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="cb852-288">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="cb852-289">偵錯記錄檔的 `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; 路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-289">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="cb852-290">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="cb852-290">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="cb852-291">`ASPNETCORE_MODULE_DEBUG` &ndash; 偵錯層級設定。</span><span class="sxs-lookup"><span data-stu-id="cb852-291">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="cb852-292">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="cb852-292">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="cb852-293">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="cb852-293">The size of the log isn't limited.</span></span> <span data-ttu-id="cb852-294">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="cb852-294">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="cb852-295">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="cb852-295">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="cb852-296">修改堆疊大小</span><span class="sxs-lookup"><span data-stu-id="cb852-296">Modify the stack size</span></span>

<span data-ttu-id="cb852-297">*僅適用于使用同進程裝載模型時。*</span><span class="sxs-lookup"><span data-stu-id="cb852-297">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="cb852-298">使用*web.config*中的 `stackSize` 設定（以位元組為單位），來設定受控堆疊大小。預設大小為 `1048576` 個位元組 (1 MB)。</span><span class="sxs-lookup"><span data-stu-id="cb852-298">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

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

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="cb852-299">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="cb852-299">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="cb852-300">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="cb852-300">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="cb852-301">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="cb852-301">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="cb852-302">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="cb852-302">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="cb852-303">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="cb852-303">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="cb852-304">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-304">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="cb852-305">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-305">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="cb852-306">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="cb852-306">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="cb852-307">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-307">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="cb852-308">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="cb852-308">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="cb852-309">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-309">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="cb852-310">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="cb852-310">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="cb852-311">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="cb852-311">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="cb852-312">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="cb852-312">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="cb852-313">在與 IIS 安裝相同的電腦上使用 IIS 共用設定時，請執行 ASP.NET Core 裝載套件組合安裝程式並將 `OPT_NO_SHARED_CONFIG_CHECK` 參數設為 `1`：</span><span class="sxs-lookup"><span data-stu-id="cb852-313">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="cb852-314">若共用設定的路徑位於與 IIS 安裝不同的電腦上，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="cb852-314">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="cb852-315">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="cb852-315">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="cb852-316">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="cb852-316">Run the installer.</span></span>
1. <span data-ttu-id="cb852-317">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="cb852-317">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="cb852-318">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="cb852-318">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="cb852-319">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="cb852-319">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="cb852-320">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="cb852-320">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="cb852-321">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="cb852-321">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="cb852-322">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cb852-322">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="cb852-323">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="cb852-323">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="cb852-324">選取 [詳細資料] 索引標籤。[檔案版本] 和 [產品版本] 代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="cb852-324">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="cb852-325">模組的「裝載套件組合」安裝程式記錄檔位於 *C:\\Users\\%UserName%\\AppData\\Local\\Temp*。檔案的名稱為 *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*。</span><span class="sxs-lookup"><span data-stu-id="cb852-325">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="cb852-326">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="cb852-326">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="cb852-327">Module</span><span class="sxs-lookup"><span data-stu-id="cb852-327">Module</span></span>

<span data-ttu-id="cb852-328">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="cb852-328">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="cb852-329">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-329">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="cb852-330">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-330">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="cb852-331">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-331">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="cb852-332">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-332">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="cb852-333">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="cb852-333">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="cb852-334">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-334">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="cb852-335">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-335">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="cb852-336">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-336">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="cb852-337">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-337">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="cb852-338">結構描述</span><span class="sxs-lookup"><span data-stu-id="cb852-338">Schema</span></span>

<span data-ttu-id="cb852-339">**IIS**</span><span class="sxs-lookup"><span data-stu-id="cb852-339">**IIS**</span></span>

* <span data-ttu-id="cb852-340">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="cb852-340">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="cb852-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="cb852-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="cb852-342">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cb852-342">**IIS Express**</span></span>

* <span data-ttu-id="cb852-343">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="cb852-343">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="cb852-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="cb852-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="cb852-345">組態</span><span class="sxs-lookup"><span data-stu-id="cb852-345">Configuration</span></span>

<span data-ttu-id="cb852-346">**IIS**</span><span class="sxs-lookup"><span data-stu-id="cb852-346">**IIS**</span></span>

* <span data-ttu-id="cb852-347">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="cb852-347">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="cb852-348">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cb852-348">**IIS Express**</span></span>

* <span data-ttu-id="cb852-349">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="cb852-349">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="cb852-350">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="cb852-350">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="cb852-351">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="cb852-351">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="cb852-352">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="cb852-352">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="cb852-353">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="cb852-353">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="cb852-354">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="cb852-354">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="cb852-355">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="cb852-355">Supported Windows versions:</span></span>

* <span data-ttu-id="cb852-356">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="cb852-356">Windows 7 or later</span></span>
* <span data-ttu-id="cb852-357">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="cb852-357">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="cb852-358">同處理序裝載時，模組會使用 IIS 的同處理序伺服程式實作，稱為 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-358">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="cb852-359">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="cb852-359">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="cb852-360">此模組[無法與 HTTP.sys 搭配運作。](xref:fundamentals/servers/httpsys)</span><span class="sxs-lookup"><span data-stu-id="cb852-360">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="cb852-361">裝載模型</span><span class="sxs-lookup"><span data-stu-id="cb852-361">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="cb852-362">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="cb852-362">In-process hosting model</span></span>

<span data-ttu-id="cb852-363">若要設定同處理序裝載的應用程式，請將 `<AspNetCoreHostingModel>` 屬性新增至應用程式的專案檔，其值為 `InProcess` (跨處理序裝載是使用 `OutOfProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="cb852-363">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="cb852-364">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="cb852-364">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="cb852-365">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="cb852-365">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="cb852-366">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="cb852-366">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="cb852-367">使用 IIS HTTP 伺服器 (`IISHttpServer`) 而不是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器。</span><span class="sxs-lookup"><span data-stu-id="cb852-367">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="cb852-368">針對同處理序，[CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 來：</span><span class="sxs-lookup"><span data-stu-id="cb852-368">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="cb852-369">註冊 `IISHttpServer`。</span><span class="sxs-lookup"><span data-stu-id="cb852-369">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="cb852-370">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-370">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="cb852-371">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="cb852-371">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="cb852-372">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="cb852-372">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="cb852-373">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="cb852-373">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="cb852-374">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="cb852-374">Use one app pool per app.</span></span>

* <span data-ttu-id="cb852-375">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="cb852-375">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="cb852-376">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="cb852-376">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="cb852-377">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="cb852-377">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="cb852-378">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="cb852-378">Client disconnects are detected.</span></span> <span data-ttu-id="cb852-379">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="cb852-379">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="cb852-380">在 ASP.NET Core 2.2.1 或更早版本中，<xref:System.IO.Directory.GetCurrentDirectory*> 會傳回 IIS 所啟動之處理序的背景工作目錄，而非應用程式的目錄 (例如 *w3wp.exe* 為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="cb852-380">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="cb852-381">如需設定應用程式目前所在目錄的範例程式碼，請參閱 [CurrentDirectoryHelpers 類別](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs)。</span><span class="sxs-lookup"><span data-stu-id="cb852-381">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="cb852-382">呼叫 `SetCurrentDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="cb852-382">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="cb852-383">後續呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 會提供應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-383">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="cb852-384">裝載同處理序時，不會內部呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 來將使用者初始化。</span><span class="sxs-lookup"><span data-stu-id="cb852-384">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="cb852-385">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="cb852-385">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="cb852-386">使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告時，請呼叫 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 來新增入驗證服務：</span><span class="sxs-lookup"><span data-stu-id="cb852-386">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="cb852-387">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="cb852-387">Out-of-process hosting model</span></span>

<span data-ttu-id="cb852-388">若要設定跨處理序裝載的應用程式，請在專案檔中使用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="cb852-388">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="cb852-389">請勿指定 `<AspNetCoreHostingModel>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-389">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="cb852-390">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="cb852-390">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="cb852-391">將 `<AspNetCoreHostingModel>` 屬性的值設定為 `OutOfProcess` (同處理序裝載是使用 `InProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="cb852-391">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="cb852-392">使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器而不是 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-392">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="cb852-393">針對跨處理序，[CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 來：</span><span class="sxs-lookup"><span data-stu-id="cb852-393">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="cb852-394">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-394">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="cb852-395">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="cb852-395">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="cb852-396">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="cb852-396">Hosting model changes</span></span>

<span data-ttu-id="cb852-397">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-397">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="cb852-398">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="cb852-398">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="cb852-399">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-399">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="cb852-400">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="cb852-400">Process name</span></span>

<span data-ttu-id="cb852-401">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="cb852-401">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="cb852-402">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="cb852-402">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="cb852-403">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="cb852-403">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="cb852-404">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="cb852-404">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="cb852-405">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-405">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="cb852-406">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="cb852-406">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="cb852-407">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="cb852-407">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="cb852-408">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="cb852-408">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="cb852-409">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="cb852-409">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="cb852-410">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="cb852-410">Configuration with web.config</span></span>

<span data-ttu-id="cb852-411">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="cb852-411">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="cb852-412">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="cb852-412">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="cb852-413">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="cb852-413">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="cb852-414">將 <xref:System.Configuration.SectionInformation.InheritInChildApplications*> 屬性設定為 `false`，以表示在 [\<位置>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 內指定的設定，不是由位在應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="cb852-414">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="cb852-415">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="cb852-415">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="cb852-416">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="cb852-416">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="cb852-417">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="cb852-417">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="cb852-418">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="cb852-418">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="cb852-419">屬性</span><span class="sxs-lookup"><span data-stu-id="cb852-419">Attribute</span></span> | <span data-ttu-id="cb852-420">描述</span><span class="sxs-lookup"><span data-stu-id="cb852-420">Description</span></span> | <span data-ttu-id="cb852-421">預設</span><span class="sxs-lookup"><span data-stu-id="cb852-421">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="cb852-422">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-422">Optional string attribute.</span></span></p><p><span data-ttu-id="cb852-423">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="cb852-423">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="cb852-424">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-424">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cb852-425">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-425">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="cb852-426">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-426">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cb852-427">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-427">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="cb852-428">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="cb852-428">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="cb852-429">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-429">Optional string attribute.</span></span></p><p><span data-ttu-id="cb852-430">將裝載模型指定為同處理序 (`InProcess`) 或跨處理序 (`OutOfProcess`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-430">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="cb852-431">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-431">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-432">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="cb852-432">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="cb852-433">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="cb852-433">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="cb852-434">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="cb852-434">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="cb852-435">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="cb852-435">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="cb852-436">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="cb852-436">Default: `1`</span></span><br><span data-ttu-id="cb852-437">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="cb852-437">Min: `1`</span></span><br><span data-ttu-id="cb852-438">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="cb852-438">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="cb852-439">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-439">Required string attribute.</span></span></p><p><span data-ttu-id="cb852-440">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-440">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="cb852-441">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-441">Relative paths are supported.</span></span> <span data-ttu-id="cb852-442">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-442">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="cb852-443">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-443">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-444">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="cb852-444">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="cb852-445">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-445">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="cb852-446">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="cb852-446">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="cb852-447">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="cb852-447">Default: `10`</span></span><br><span data-ttu-id="cb852-448">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="cb852-448">Min: `0`</span></span><br><span data-ttu-id="cb852-449">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="cb852-449">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="cb852-450">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-450">Optional timespan attribute.</span></span></p><p><span data-ttu-id="cb852-451">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="cb852-451">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="cb852-452">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="cb852-452">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="cb852-453">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="cb852-453">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="cb852-454">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-454">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="cb852-455">字串之分鐘和秒數的有效值介於 0-59。</span><span class="sxs-lookup"><span data-stu-id="cb852-455">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="cb852-456">在分鐘或秒數的值中使用 **60** 將會導致「500 - 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="cb852-456">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="cb852-457">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="cb852-457">Default: `00:02:00`</span></span><br><span data-ttu-id="cb852-458">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="cb852-458">Min: `00:00:00`</span></span><br><span data-ttu-id="cb852-459">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="cb852-459">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="cb852-460">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-460">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-461">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="cb852-461">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="cb852-462">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="cb852-462">Default: `10`</span></span><br><span data-ttu-id="cb852-463">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="cb852-463">Min: `0`</span></span><br><span data-ttu-id="cb852-464">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="cb852-464">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="cb852-465">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-465">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-466">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="cb852-466">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="cb852-467">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-467">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="cb852-468">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="cb852-468">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="cb852-469">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="cb852-469">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="cb852-470">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="cb852-470">Default: `120`</span></span><br><span data-ttu-id="cb852-471">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="cb852-471">Min: `0`</span></span><br><span data-ttu-id="cb852-472">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="cb852-472">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="cb852-473">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-473">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cb852-474">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="cb852-474">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="cb852-475">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-475">Optional string attribute.</span></span></p><p><span data-ttu-id="cb852-476">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-476">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="cb852-477">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-477">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="cb852-478">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-478">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="cb852-479">建立記錄檔後，模組會建立路徑中提供的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="cb852-479">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="cb852-480">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 ( *.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="cb852-480">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="cb852-481">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="cb852-481">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="cb852-482">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="cb852-482">Setting environment variables</span></span>

<span data-ttu-id="cb852-483">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-483">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="cb852-484">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-484">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="cb852-485">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-485">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="cb852-486">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-486">The following example sets two environment variables.</span></span> <span data-ttu-id="cb852-487">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="cb852-487">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="cb852-488">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="cb852-488">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="cb852-489">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-489">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="cb852-490">在 *web.config* 中直接設定環境的替代方式是在發行設定檔 ( *.pubxml*) 或專案檔中包括 `<EnvironmentName>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-490">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="cb852-491">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="cb852-491">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="cb852-492">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="cb852-492">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="cb852-493">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="cb852-493">app_offline.htm</span></span>

<span data-ttu-id="cb852-494">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-494">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="cb852-495">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-495">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="cb852-496">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-496">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="cb852-497">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb852-497">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="cb852-498">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="cb852-498">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="cb852-499">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="cb852-499">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="cb852-500">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="cb852-500">Start-up error page</span></span>

<span data-ttu-id="cb852-501">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-501">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="cb852-502">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-502">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="cb852-503">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-503">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="cb852-504">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-504">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="cb852-505">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-505">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="cb852-506">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="cb852-506">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="cb852-507">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="cb852-507">Log creation and redirection</span></span>

<span data-ttu-id="cb852-508">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="cb852-508">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="cb852-509">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="cb852-509">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="cb852-510">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="cb852-510">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="cb852-511">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-511">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="cb852-512">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="cb852-512">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="cb852-513">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-513">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="cb852-514">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-514">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="cb852-515">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="cb852-515">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="cb852-516">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="cb852-516">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="cb852-517">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="cb852-517">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="cb852-518">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 ( *.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="cb852-518">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="cb852-519">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="cb852-519">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="cb852-520">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-520">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="cb852-521">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-521">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="cb852-522">下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-522">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="cb852-523">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-523">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="cb852-524">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="cb852-524">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="cb852-525">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="cb852-525">Enhanced diagnostic logs</span></span>

<span data-ttu-id="cb852-526">ASP.NET Core 模組是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-526">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="cb852-527">將 `<handlerSettings>` 項目新增至 *web.config* 中的 `<aspNetCore>` 項目。將 `debugLevel` 設定為 `TRACE` 會公開精確性更高的診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="cb852-527">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="cb852-528">模組不會在提供給 `<handlerSetting>` 值的路徑 (在上述範例中為 *logs*) 中自動建立資料夾，而且這些資料夾應該已存在於部署中。</span><span class="sxs-lookup"><span data-stu-id="cb852-528">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="cb852-529">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="cb852-529">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="cb852-530">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="cb852-530">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="cb852-531">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="cb852-531">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="cb852-532">ERROR</span><span class="sxs-lookup"><span data-stu-id="cb852-532">ERROR</span></span>
* <span data-ttu-id="cb852-533">WARNING</span><span class="sxs-lookup"><span data-stu-id="cb852-533">WARNING</span></span>
* <span data-ttu-id="cb852-534">INFO</span><span class="sxs-lookup"><span data-stu-id="cb852-534">INFO</span></span>
* <span data-ttu-id="cb852-535">TRACE</span><span class="sxs-lookup"><span data-stu-id="cb852-535">TRACE</span></span>

<span data-ttu-id="cb852-536">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="cb852-536">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="cb852-537">主控台</span><span class="sxs-lookup"><span data-stu-id="cb852-537">CONSOLE</span></span>
* <span data-ttu-id="cb852-538">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="cb852-538">EVENTLOG</span></span>
* <span data-ttu-id="cb852-539">檔案</span><span class="sxs-lookup"><span data-stu-id="cb852-539">FILE</span></span>

<span data-ttu-id="cb852-540">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="cb852-540">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="cb852-541">偵錯記錄檔的 `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; 路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-541">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="cb852-542">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="cb852-542">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="cb852-543">`ASPNETCORE_MODULE_DEBUG` &ndash; 偵錯層級設定。</span><span class="sxs-lookup"><span data-stu-id="cb852-543">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="cb852-544">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="cb852-544">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="cb852-545">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="cb852-545">The size of the log isn't limited.</span></span> <span data-ttu-id="cb852-546">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="cb852-546">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="cb852-547">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="cb852-547">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="cb852-548">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="cb852-548">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="cb852-549">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="cb852-549">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="cb852-550">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="cb852-550">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="cb852-551">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="cb852-551">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="cb852-552">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="cb852-552">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="cb852-553">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-553">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="cb852-554">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-554">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="cb852-555">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="cb852-555">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="cb852-556">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-556">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="cb852-557">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="cb852-557">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="cb852-558">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-558">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="cb852-559">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="cb852-559">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="cb852-560">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="cb852-560">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="cb852-561">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="cb852-561">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="cb852-562">在與 IIS 安裝相同的電腦上使用 IIS 共用設定時，請執行 ASP.NET Core 裝載套件組合安裝程式並將 `OPT_NO_SHARED_CONFIG_CHECK` 參數設為 `1`：</span><span class="sxs-lookup"><span data-stu-id="cb852-562">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="cb852-563">若共用設定的路徑位於與 IIS 安裝不同的電腦上，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="cb852-563">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="cb852-564">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="cb852-564">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="cb852-565">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="cb852-565">Run the installer.</span></span>
1. <span data-ttu-id="cb852-566">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="cb852-566">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="cb852-567">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="cb852-567">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="cb852-568">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="cb852-568">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="cb852-569">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="cb852-569">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="cb852-570">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="cb852-570">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="cb852-571">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cb852-571">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="cb852-572">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="cb852-572">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="cb852-573">選取 [詳細資料] 索引標籤。[檔案版本] 和 [產品版本] 代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="cb852-573">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="cb852-574">模組的「裝載套件組合」安裝程式記錄檔位於 *C:\\Users\\%UserName%\\AppData\\Local\\Temp*。檔案的名稱為 *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*。</span><span class="sxs-lookup"><span data-stu-id="cb852-574">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="cb852-575">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="cb852-575">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="cb852-576">Module</span><span class="sxs-lookup"><span data-stu-id="cb852-576">Module</span></span>

<span data-ttu-id="cb852-577">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="cb852-577">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="cb852-578">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-578">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="cb852-579">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-579">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="cb852-580">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-580">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="cb852-581">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-581">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="cb852-582">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="cb852-582">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="cb852-583">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-583">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="cb852-584">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-584">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="cb852-585">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-585">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="cb852-586">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-586">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="cb852-587">結構描述</span><span class="sxs-lookup"><span data-stu-id="cb852-587">Schema</span></span>

<span data-ttu-id="cb852-588">**IIS**</span><span class="sxs-lookup"><span data-stu-id="cb852-588">**IIS**</span></span>

* <span data-ttu-id="cb852-589">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="cb852-589">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="cb852-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="cb852-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="cb852-591">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cb852-591">**IIS Express**</span></span>

* <span data-ttu-id="cb852-592">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="cb852-592">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="cb852-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="cb852-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="cb852-594">組態</span><span class="sxs-lookup"><span data-stu-id="cb852-594">Configuration</span></span>

<span data-ttu-id="cb852-595">**IIS**</span><span class="sxs-lookup"><span data-stu-id="cb852-595">**IIS**</span></span>

* <span data-ttu-id="cb852-596">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="cb852-596">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="cb852-597">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cb852-597">**IIS Express**</span></span>

* <span data-ttu-id="cb852-598">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="cb852-598">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="cb852-599">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="cb852-599">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="cb852-600">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="cb852-600">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="cb852-601">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線，將 Web 要求重新轉送到後端的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb852-601">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="cb852-602">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="cb852-602">Supported Windows versions:</span></span>

* <span data-ttu-id="cb852-603">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="cb852-603">Windows 7 or later</span></span>
* <span data-ttu-id="cb852-604">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="cb852-604">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="cb852-605">該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="cb852-605">The module only works with Kestrel.</span></span> <span data-ttu-id="cb852-606">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) 不相容。</span><span class="sxs-lookup"><span data-stu-id="cb852-606">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="cb852-607">因為處理序中執行的 ASP.NET Core 應用程式會與 IIS 背景工作處理序分開，所以此模組也會執行處理序管理。</span><span class="sxs-lookup"><span data-stu-id="cb852-607">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="cb852-608">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="cb852-608">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="cb852-609">此行為基本上與在 IIS 中執行同處理序，並由 [Windows 處理器啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的 ASP.NET 4.x 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="cb852-609">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="cb852-610">下圖說明 IIS、ASP.NET Core 模組和應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="cb852-610">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core 模組](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="cb852-612">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="cb852-612">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="cb852-613">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="cb852-613">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="cb852-614">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="cb852-614">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="cb852-615">此模組在啟動時透過環境變數指定連接埠，而 [IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="cb852-615">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="cb852-616">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="cb852-616">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="cb852-617">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="cb852-617">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="cb852-618">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="cb852-618">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="cb852-619">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="cb852-619">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="cb852-620">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="cb852-620">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="cb852-621">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="cb852-621">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="cb852-622">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="cb852-622">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="cb852-623">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="cb852-623">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="cb852-624">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="cb852-624">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="cb852-625">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-625">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="cb852-626">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="cb852-626">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="cb852-627">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="cb852-627">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="cb852-628">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="cb852-628">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="cb852-629">如需如何安裝 ASP.NET Core 模組的指示，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="cb852-629">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="cb852-630">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="cb852-630">Configuration with web.config</span></span>

<span data-ttu-id="cb852-631">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="cb852-631">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="cb852-632">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="cb852-632">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="cb852-633">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="cb852-633">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="cb852-634">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="cb852-634">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="cb852-635">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="cb852-635">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="cb852-636">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="cb852-636">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="cb852-637">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="cb852-637">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="cb852-638">屬性</span><span class="sxs-lookup"><span data-stu-id="cb852-638">Attribute</span></span> | <span data-ttu-id="cb852-639">描述</span><span class="sxs-lookup"><span data-stu-id="cb852-639">Description</span></span> | <span data-ttu-id="cb852-640">預設</span><span class="sxs-lookup"><span data-stu-id="cb852-640">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="cb852-641">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-641">Optional string attribute.</span></span></p><p><span data-ttu-id="cb852-642">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="cb852-642">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="cb852-643">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-643">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cb852-644">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-644">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="cb852-645">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-645">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cb852-646">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-646">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="cb852-647">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="cb852-647">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="cb852-648">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-648">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-649">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="cb852-649">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="cb852-650">不建議使用 `processesPerApplication` 設定。</span><span class="sxs-lookup"><span data-stu-id="cb852-650">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="cb852-651">此屬性將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="cb852-651">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="cb852-652">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="cb852-652">Default: `1`</span></span><br><span data-ttu-id="cb852-653">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="cb852-653">Min: `1`</span></span><br><span data-ttu-id="cb852-654">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="cb852-654">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="cb852-655">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-655">Required string attribute.</span></span></p><p><span data-ttu-id="cb852-656">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-656">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="cb852-657">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-657">Relative paths are supported.</span></span> <span data-ttu-id="cb852-658">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-658">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="cb852-659">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-659">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-660">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="cb852-660">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="cb852-661">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-661">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="cb852-662">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="cb852-662">Default: `10`</span></span><br><span data-ttu-id="cb852-663">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="cb852-663">Min: `0`</span></span><br><span data-ttu-id="cb852-664">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="cb852-664">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="cb852-665">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-665">Optional timespan attribute.</span></span></p><p><span data-ttu-id="cb852-666">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="cb852-666">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="cb852-667">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="cb852-667">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="cb852-668">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="cb852-668">Default: `00:02:00`</span></span><br><span data-ttu-id="cb852-669">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="cb852-669">Min: `00:00:00`</span></span><br><span data-ttu-id="cb852-670">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="cb852-670">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="cb852-671">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-671">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-672">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="cb852-672">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="cb852-673">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="cb852-673">Default: `10`</span></span><br><span data-ttu-id="cb852-674">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="cb852-674">Min: `0`</span></span><br><span data-ttu-id="cb852-675">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="cb852-675">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="cb852-676">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-676">Optional integer attribute.</span></span></p><p><span data-ttu-id="cb852-677">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="cb852-677">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="cb852-678">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-678">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="cb852-679">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="cb852-679">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="cb852-680">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="cb852-680">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="cb852-681">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="cb852-681">Default: `120`</span></span><br><span data-ttu-id="cb852-682">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="cb852-682">Min: `0`</span></span><br><span data-ttu-id="cb852-683">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="cb852-683">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="cb852-684">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-684">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cb852-685">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="cb852-685">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="cb852-686">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-686">Optional string attribute.</span></span></p><p><span data-ttu-id="cb852-687">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-687">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="cb852-688">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-688">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="cb852-689">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-689">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="cb852-690">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-690">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="cb852-691">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 ( *.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="cb852-691">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="cb852-692">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="cb852-692">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="cb852-693">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="cb852-693">Setting environment variables</span></span>

<span data-ttu-id="cb852-694">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-694">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="cb852-695">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-695">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="cb852-696">此節中所設定的環境變數與使用相同名稱設定的系統環境變數相衝突。</span><span class="sxs-lookup"><span data-stu-id="cb852-696">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="cb852-697">若同時在 *web.config* 檔案與 Windows 中的系統層級設定環境變數，來自 *web.config* 檔案的值會成為附加到系統環境變數值 (例如，`ASPNETCORE_ENVIRONMENT: Development;Development`)，這會造成應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="cb852-697">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="cb852-698">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="cb852-698">The following example sets two environment variables.</span></span> <span data-ttu-id="cb852-699">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="cb852-699">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="cb852-700">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="cb852-700">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="cb852-701">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="cb852-701">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="cb852-702">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="cb852-702">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="cb852-703">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="cb852-703">app_offline.htm</span></span>

<span data-ttu-id="cb852-704">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-704">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="cb852-705">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="cb852-705">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="cb852-706">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-706">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="cb852-707">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb852-707">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="cb852-708">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="cb852-708">Start-up error page</span></span>

<span data-ttu-id="cb852-709">如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cb852-709">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="cb852-710">若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cb852-710">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="cb852-711">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="cb852-711">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="cb852-713">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="cb852-713">Log creation and redirection</span></span>

<span data-ttu-id="cb852-714">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="cb852-714">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="cb852-715">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="cb852-715">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="cb852-716">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="cb852-716">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="cb852-717">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-717">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="cb852-718">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="cb852-718">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="cb852-719">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cb852-719">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="cb852-720">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-720">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="cb852-721">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="cb852-721">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="cb852-722">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="cb852-722">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="cb852-723">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="cb852-723">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="cb852-724">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 ( *.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="cb852-724">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="cb852-725">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="cb852-725">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="cb852-726">下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-726">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="cb852-727">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="cb852-727">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="cb852-728">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="cb852-728">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="cb852-729">模組不會在提供給 `<handlerSetting>` 值的路徑 (在上述範例中為 *logs*) 中自動建立資料夾，而且這些資料夾應該已存在於部署中。</span><span class="sxs-lookup"><span data-stu-id="cb852-729">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="cb852-730">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="cb852-730">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="cb852-731">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="cb852-731">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="cb852-732">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="cb852-732">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="cb852-733">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="cb852-733">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="cb852-734">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="cb852-734">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="cb852-735">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="cb852-735">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="cb852-736">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-736">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="cb852-737">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="cb852-737">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="cb852-738">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="cb852-738">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="cb852-739">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-739">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="cb852-740">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="cb852-740">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="cb852-741">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="cb852-741">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="cb852-742">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="cb852-742">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="cb852-743">ASP.NET Core 模組安裝程式會以 **TrustedInstaller** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="cb852-743">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="cb852-744">由於本機系統帳戶並未具備 IIS 共用設定所使用的共用路徑修改權限，因此，安裝程式在嘗試於共用上的 *applicationHost.config* 檔案中進行模組設定時，會擲回拒絕存取的錯誤。</span><span class="sxs-lookup"><span data-stu-id="cb852-744">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="cb852-745">使用「IIS 共用設定」時，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="cb852-745">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="cb852-746">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="cb852-746">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="cb852-747">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="cb852-747">Run the installer.</span></span>
1. <span data-ttu-id="cb852-748">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="cb852-748">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="cb852-749">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="cb852-749">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="cb852-750">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="cb852-750">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="cb852-751">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="cb852-751">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="cb852-752">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="cb852-752">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="cb852-753">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cb852-753">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="cb852-754">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="cb852-754">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="cb852-755">選取 [詳細資料] 索引標籤。[檔案版本] 和 [產品版本] 代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="cb852-755">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="cb852-756">模組的「裝載套件組合」安裝程式記錄檔位於 *C:\\Users\\%UserName%\\AppData\\Local\\Temp*。檔案的名稱為 *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*。</span><span class="sxs-lookup"><span data-stu-id="cb852-756">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="cb852-757">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="cb852-757">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="cb852-758">Module</span><span class="sxs-lookup"><span data-stu-id="cb852-758">Module</span></span>

<span data-ttu-id="cb852-759">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="cb852-759">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="cb852-760">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-760">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="cb852-761">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-761">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="cb852-762">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="cb852-762">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="cb852-763">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-763">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="cb852-764">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cb852-764">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="cb852-765">結構描述</span><span class="sxs-lookup"><span data-stu-id="cb852-765">Schema</span></span>

<span data-ttu-id="cb852-766">**IIS**</span><span class="sxs-lookup"><span data-stu-id="cb852-766">**IIS**</span></span>

* <span data-ttu-id="cb852-767">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="cb852-767">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="cb852-768">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cb852-768">**IIS Express**</span></span>

* <span data-ttu-id="cb852-769">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="cb852-769">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="cb852-770">組態</span><span class="sxs-lookup"><span data-stu-id="cb852-770">Configuration</span></span>

<span data-ttu-id="cb852-771">**IIS**</span><span class="sxs-lookup"><span data-stu-id="cb852-771">**IIS**</span></span>

* <span data-ttu-id="cb852-772">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="cb852-772">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="cb852-773">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cb852-773">**IIS Express**</span></span>

* <span data-ttu-id="cb852-774">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="cb852-774">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="cb852-775">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="cb852-775">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="cb852-776">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="cb852-776">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="cb852-777">其他資源</span><span class="sxs-lookup"><span data-stu-id="cb852-777">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="cb852-778">ASP.NET Core 模組 GitHub 存放庫 (參考來源)</span><span class="sxs-lookup"><span data-stu-id="cb852-778">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
