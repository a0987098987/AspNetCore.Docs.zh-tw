---
title: ASP.NET Core 模組設定參考
author: guardrex
description: 了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: ca86b1548c7c28a64fd391617b2e8290c1c264cf
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191356"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="09152-103">ASP.NET Core 模組設定參考</span><span class="sxs-lookup"><span data-stu-id="09152-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="09152-104">作者：[Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti) 及 [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="09152-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="09152-105">本文說明如何設定 ASP.NET Core 模組來裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="09152-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="09152-106">如需 ASP.NET Core 模組簡介及安裝指示，請參閱 [ASP.NET Core 模組概觀](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="09152-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="09152-107">裝載模型</span><span class="sxs-lookup"><span data-stu-id="09152-107">Hosting model</span></span>

<span data-ttu-id="09152-108">針對執行 .NET Core 2.2 或更新版本的應用程式，該模組支援同處理序裝載模型，因此相較於反向 Proxy (跨處理序) 裝載會有更高的效能。</span><span class="sxs-lookup"><span data-stu-id="09152-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="09152-109">如需詳細資訊，請參閱<xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>。</span><span class="sxs-lookup"><span data-stu-id="09152-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="09152-110">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會針對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="09152-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="09152-111">若要設定同處理序裝載的應用程式，請將 `<AspNetCoreHostingModel>` 屬性新增至應用程式的專案檔 (例如 *MyApp.csproj*)，且其值為 `inprocess` (跨處理序裝載是使用 `outofprocess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="09152-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="09152-112">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="09152-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="09152-113">不會使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="09152-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="09152-114">自訂 <xref:Microsoft.AspNetCore.Hosting.Server.IServer> 實作 `IISHttpServer` 可作為應用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="09152-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="09152-115">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="09152-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="09152-116">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="09152-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="09152-117">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="09152-117">Use one app pool per app.</span></span>

* <span data-ttu-id="09152-118">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="09152-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="09152-119">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="09152-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="09152-120">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="09152-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="09152-121">如果使用 `WebHostBuilder` (而不是使用 [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) 以手動方式設定應用程式的主機，而且曾在 Kestrel 伺服器上直接執行應用程式 (自我裝載)，請先呼叫 `UseKestrel`，再呼叫 `UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="09152-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="09152-122">如果順序相反，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="09152-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="09152-123">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="09152-123">Client disconnects are detected.</span></span> <span data-ttu-id="09152-124">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="09152-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="09152-125">`Directory.GetCurrentDirectory()` 會傳回 IIS 所啟動處理序的背景工作目錄，而非應用程式目錄 (例如 *w3wp.exe*為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="09152-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="09152-126">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="09152-126">Hosting model changes</span></span>

<span data-ttu-id="09152-127">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="09152-128">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="09152-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="09152-129">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="09152-130">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="09152-130">Process name</span></span>

<span data-ttu-id="09152-131">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="09152-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="09152-132">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="09152-132">Configuration with web.config</span></span>

<span data-ttu-id="09152-133">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="09152-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="09152-134">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="09152-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

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

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

::: moniker-end

<span data-ttu-id="09152-135">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="09152-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

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

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

::: moniker-end

<span data-ttu-id="09152-136">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="09152-136">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="09152-137">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="09152-137">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="09152-138">如需子應用程式中 *web.config* 檔案設定相關的重要注意事項，請參閱[子應用程式設定](xref:host-and-deploy/iis/index#sub-application-configuration)。</span><span class="sxs-lookup"><span data-stu-id="09152-138">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="09152-139">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="09152-139">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="09152-140">屬性</span><span class="sxs-lookup"><span data-stu-id="09152-140">Attribute</span></span> | <span data-ttu-id="09152-141">描述</span><span class="sxs-lookup"><span data-stu-id="09152-141">Description</span></span> | <span data-ttu-id="09152-142">預設</span><span class="sxs-lookup"><span data-stu-id="09152-142">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="09152-143">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-143">Optional string attribute.</span></span></p><p><span data-ttu-id="09152-144">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="09152-144">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="09152-145">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-145">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09152-146">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="09152-146">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="09152-147">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-147">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09152-148">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-148">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="09152-149">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="09152-149">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="09152-150">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-150">Optional string attribute.</span></span></p><p><span data-ttu-id="09152-151">將裝載模型指定為同處理序 (`inprocess`) 或跨處理序 (`outofprocess`)。</span><span class="sxs-lookup"><span data-stu-id="09152-151">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="09152-152">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-152">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-153">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="09152-153">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="09152-154">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="09152-154">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="09152-155">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="09152-155">Default: `1`</span></span><br><span data-ttu-id="09152-156">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="09152-156">Min: `1`</span></span><br><span data-ttu-id="09152-157">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="09152-157">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="09152-158">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-158">Required string attribute.</span></span></p><p><span data-ttu-id="09152-159">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-159">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="09152-160">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-160">Relative paths are supported.</span></span> <span data-ttu-id="09152-161">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-161">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="09152-162">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-162">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-163">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="09152-163">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="09152-164">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-164">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="09152-165">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="09152-165">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="09152-166">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="09152-166">Default: `10`</span></span><br><span data-ttu-id="09152-167">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="09152-167">Min: `0`</span></span><br><span data-ttu-id="09152-168">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="09152-168">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="09152-169">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-169">Optional timespan attribute.</span></span></p><p><span data-ttu-id="09152-170">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="09152-170">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="09152-171">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="09152-171">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="09152-172">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="09152-172">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="09152-173">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="09152-173">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="09152-174">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="09152-174">Default: `00:02:00`</span></span><br><span data-ttu-id="09152-175">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="09152-175">Min: `00:00:00`</span></span><br><span data-ttu-id="09152-176">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="09152-176">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="09152-177">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-177">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-178">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="09152-178">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="09152-179">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="09152-179">Default: `10`</span></span><br><span data-ttu-id="09152-180">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="09152-180">Min: `0`</span></span><br><span data-ttu-id="09152-181">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="09152-181">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="09152-182">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-183">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="09152-183">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="09152-184">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-184">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="09152-185">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="09152-185">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="09152-186">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="09152-186">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="09152-187">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="09152-187">Default: `120`</span></span><br><span data-ttu-id="09152-188">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="09152-188">Min: `0`</span></span><br><span data-ttu-id="09152-189">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="09152-189">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="09152-190">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09152-191">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="09152-191">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="09152-192">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-192">Optional string attribute.</span></span></p><p><span data-ttu-id="09152-193">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-193">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="09152-194">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="09152-194">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="09152-195">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-195">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="09152-196">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="09152-196">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="09152-197">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="09152-197">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="09152-198">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="09152-198">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="09152-199">屬性</span><span class="sxs-lookup"><span data-stu-id="09152-199">Attribute</span></span> | <span data-ttu-id="09152-200">描述</span><span class="sxs-lookup"><span data-stu-id="09152-200">Description</span></span> | <span data-ttu-id="09152-201">預設</span><span class="sxs-lookup"><span data-stu-id="09152-201">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="09152-202">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-202">Optional string attribute.</span></span></p><p><span data-ttu-id="09152-203">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="09152-203">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="09152-204">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-204">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09152-205">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="09152-205">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="09152-206">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-206">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09152-207">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-207">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="09152-208">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="09152-208">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="09152-209">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-209">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-210">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="09152-210">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="09152-211">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="09152-211">Default: `1`</span></span><br><span data-ttu-id="09152-212">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="09152-212">Min: `1`</span></span><br><span data-ttu-id="09152-213">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="09152-213">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="09152-214">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-214">Required string attribute.</span></span></p><p><span data-ttu-id="09152-215">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-215">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="09152-216">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-216">Relative paths are supported.</span></span> <span data-ttu-id="09152-217">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-217">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="09152-218">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-218">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-219">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="09152-219">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="09152-220">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-220">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="09152-221">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="09152-221">Default: `10`</span></span><br><span data-ttu-id="09152-222">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="09152-222">Min: `0`</span></span><br><span data-ttu-id="09152-223">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="09152-223">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="09152-224">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-224">Optional timespan attribute.</span></span></p><p><span data-ttu-id="09152-225">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="09152-225">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="09152-226">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="09152-226">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="09152-227">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="09152-227">Default: `00:02:00`</span></span><br><span data-ttu-id="09152-228">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="09152-228">Min: `00:00:00`</span></span><br><span data-ttu-id="09152-229">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="09152-229">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="09152-230">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-230">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-231">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="09152-231">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="09152-232">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="09152-232">Default: `10`</span></span><br><span data-ttu-id="09152-233">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="09152-233">Min: `0`</span></span><br><span data-ttu-id="09152-234">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="09152-234">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="09152-235">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-235">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-236">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="09152-236">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="09152-237">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-237">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="09152-238">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="09152-238">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="09152-239">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="09152-239">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="09152-240">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="09152-240">Default: `120`</span></span><br><span data-ttu-id="09152-241">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="09152-241">Min: `0`</span></span><br><span data-ttu-id="09152-242">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="09152-242">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="09152-243">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-243">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09152-244">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="09152-244">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="09152-245">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-245">Optional string attribute.</span></span></p><p><span data-ttu-id="09152-246">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-246">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="09152-247">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="09152-247">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="09152-248">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-248">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="09152-249">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="09152-249">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="09152-250">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="09152-250">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="09152-251">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="09152-251">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="09152-252">屬性</span><span class="sxs-lookup"><span data-stu-id="09152-252">Attribute</span></span> | <span data-ttu-id="09152-253">描述</span><span class="sxs-lookup"><span data-stu-id="09152-253">Description</span></span> | <span data-ttu-id="09152-254">預設</span><span class="sxs-lookup"><span data-stu-id="09152-254">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="09152-255">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-255">Optional string attribute.</span></span></p><p><span data-ttu-id="09152-256">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="09152-256">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="09152-257">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-257">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09152-258">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="09152-258">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="09152-259">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-259">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09152-260">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-260">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="09152-261">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="09152-261">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="09152-262">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-262">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-263">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="09152-263">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="09152-264">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="09152-264">Default: `1`</span></span><br><span data-ttu-id="09152-265">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="09152-265">Min: `1`</span></span><br><span data-ttu-id="09152-266">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="09152-266">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="09152-267">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-267">Required string attribute.</span></span></p><p><span data-ttu-id="09152-268">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-268">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="09152-269">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-269">Relative paths are supported.</span></span> <span data-ttu-id="09152-270">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-270">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="09152-271">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-271">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-272">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="09152-272">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="09152-273">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-273">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="09152-274">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="09152-274">Default: `10`</span></span><br><span data-ttu-id="09152-275">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="09152-275">Min: `0`</span></span><br><span data-ttu-id="09152-276">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="09152-276">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="09152-277">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-277">Optional timespan attribute.</span></span></p><p><span data-ttu-id="09152-278">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="09152-278">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="09152-279">在 ASP.NET Core 2.0 或更舊版本隨附的 ASP.NET Core 模組版本中，只能以整數分鐘為單位來指定 `requestTimeout`，否則會預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="09152-279">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="09152-280">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="09152-280">Default: `00:02:00`</span></span><br><span data-ttu-id="09152-281">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="09152-281">Min: `00:00:00`</span></span><br><span data-ttu-id="09152-282">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="09152-282">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="09152-283">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-283">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-284">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="09152-284">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="09152-285">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="09152-285">Default: `10`</span></span><br><span data-ttu-id="09152-286">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="09152-286">Min: `0`</span></span><br><span data-ttu-id="09152-287">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="09152-287">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="09152-288">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-288">Optional integer attribute.</span></span></p><p><span data-ttu-id="09152-289">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="09152-289">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="09152-290">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-290">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="09152-291">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="09152-291">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="09152-292">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="09152-292">Default: `120`</span></span><br><span data-ttu-id="09152-293">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="09152-293">Min: `0`</span></span><br><span data-ttu-id="09152-294">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="09152-294">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="09152-295">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-295">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="09152-296">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="09152-296">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="09152-297">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-297">Optional string attribute.</span></span></p><p><span data-ttu-id="09152-298">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-298">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="09152-299">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="09152-299">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="09152-300">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-300">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="09152-301">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="09152-301">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="09152-302">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="09152-302">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="09152-303">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="09152-303">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="09152-304">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="09152-304">Setting environment variables</span></span>

<span data-ttu-id="09152-305">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="09152-305">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="09152-306">請使用 `environmentVariables` 集合元素的 `environmentVariable` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="09152-306">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="09152-307">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="09152-307">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="09152-308">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="09152-308">The following example sets two environment variables.</span></span> <span data-ttu-id="09152-309">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="09152-309">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="09152-310">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="09152-310">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="09152-311">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-311">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

::: moniker-end

> [!WARNING]
> <span data-ttu-id="09152-312">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="09152-312">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="09152-313">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="09152-313">app_offline.htm</span></span>

<span data-ttu-id="09152-314">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="09152-314">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="09152-315">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="09152-315">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="09152-316">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="09152-316">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="09152-317">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="09152-317">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="09152-318">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="09152-318">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="09152-319">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="09152-319">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="09152-320">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="09152-320">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="09152-321">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="09152-321">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="09152-322">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="09152-322">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="09152-323">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="09152-323">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="09152-324">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="09152-324">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="09152-325">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-325">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="09152-326">如需有關設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="09152-326">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="09152-327">如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="09152-327">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="09152-328">若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="09152-328">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="09152-329">如需有關設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="09152-329">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="09152-331">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="09152-331">Log creation and redirection</span></span>

<span data-ttu-id="09152-332">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="09152-332">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="09152-333">`stdoutLogFile` 路徑中的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="09152-333">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="09152-334">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="09152-334">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="09152-335">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="09152-335">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="09152-336">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="09152-336">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="09152-337">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="09152-337">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="09152-338">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="09152-338">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="09152-339">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="09152-339">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="09152-340">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="09152-340">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="09152-341">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="09152-341">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="09152-342">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 (*.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="09152-342">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="09152-343">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="09152-343">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="09152-344">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="09152-344">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="09152-345">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="09152-345">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="09152-346">下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="09152-346">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="09152-347">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="09152-347">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="09152-348">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="09152-348">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="09152-349">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="09152-349">Enhanced diagnostic logs</span></span>

<span data-ttu-id="09152-350">ASP.NET Core 模組提供者是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="09152-350">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="09152-351">將 `<handlerSettings>` 項目新增至 *web.config* 中的 `<aspNetCore>` 項目。將 `debugLevel` 設定為 `TRACE` 會公開精確性更高的診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="09152-351">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="09152-352">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="09152-352">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="09152-353">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="09152-353">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="09152-354">ERROR</span><span class="sxs-lookup"><span data-stu-id="09152-354">ERROR</span></span>
* <span data-ttu-id="09152-355">WARNING</span><span class="sxs-lookup"><span data-stu-id="09152-355">WARNING</span></span>
* <span data-ttu-id="09152-356">INFO</span><span class="sxs-lookup"><span data-stu-id="09152-356">INFO</span></span>
* <span data-ttu-id="09152-357">TRACE</span><span class="sxs-lookup"><span data-stu-id="09152-357">TRACE</span></span>

<span data-ttu-id="09152-358">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="09152-358">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="09152-359">主控台</span><span class="sxs-lookup"><span data-stu-id="09152-359">CONSOLE</span></span>
* <span data-ttu-id="09152-360">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="09152-360">EVENTLOG</span></span>
* <span data-ttu-id="09152-361">檔案</span><span class="sxs-lookup"><span data-stu-id="09152-361">FILE</span></span>

<span data-ttu-id="09152-362">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="09152-362">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="09152-363">偵錯記錄檔的 `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; 路徑。</span><span class="sxs-lookup"><span data-stu-id="09152-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="09152-364">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="09152-364">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="09152-365">`ASPNETCORE_MODULE_DEBUG` &ndash; 偵錯層級設定。</span><span class="sxs-lookup"><span data-stu-id="09152-365">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="09152-366">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="09152-366">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="09152-367">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="09152-367">The size of the log isn't limited.</span></span> <span data-ttu-id="09152-368">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="09152-368">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="09152-369">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="09152-369">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="09152-370">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="09152-370">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="09152-371">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="09152-371">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="09152-372">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="09152-372">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="09152-373">使用 HTTP 是一項效能最佳化作業，其中模組與 Kestrel 之間的流量會在網路介面外的回送位址進行。</span><span class="sxs-lookup"><span data-stu-id="09152-373">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="09152-374">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="09152-374">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="09152-375">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="09152-375">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="09152-376">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="09152-376">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="09152-377">配對權杖也會設定成每個代理要求的標頭 (`MSAspNetCoreToken`)。</span><span class="sxs-lookup"><span data-stu-id="09152-377">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="09152-378">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="09152-378">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="09152-379">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="09152-379">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="09152-380">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="09152-380">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="09152-381">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="09152-381">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="09152-382">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="09152-382">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="09152-383">ASP.NET Core 模組安裝程式會以 **SYSTEM** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="09152-383">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="09152-384">由於本機系統帳戶並不具備「IIS 共用設定」所使用共用路徑的修改權限，因此安裝程式在嘗試於共用上的 *applicationHost.config* 中進行模組設定時，會發生存取被拒錯誤。</span><span class="sxs-lookup"><span data-stu-id="09152-384">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="09152-385">使用「IIS 共用設定」時，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="09152-385">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="09152-386">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="09152-386">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="09152-387">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="09152-387">Run the installer.</span></span>
1. <span data-ttu-id="09152-388">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="09152-388">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="09152-389">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="09152-389">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="09152-390">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="09152-390">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="09152-391">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="09152-391">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="09152-392">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="09152-392">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="09152-393">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="09152-393">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="09152-394">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="09152-394">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="09152-395">選取 [詳細資料] 索引標籤。[檔案版本] 和 [產品版本] 代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="09152-395">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="09152-396">模組的「裝載套件組合」安裝程式記錄檔位於 *C:\\Users\\%UserName%\\AppData\\Local\\Temp*。檔案的名稱為 *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*。</span><span class="sxs-lookup"><span data-stu-id="09152-396">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="09152-397">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="09152-397">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="09152-398">Module</span><span class="sxs-lookup"><span data-stu-id="09152-398">Module</span></span>

<span data-ttu-id="09152-399">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="09152-399">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="09152-400">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="09152-400">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="09152-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="09152-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="09152-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="09152-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="09152-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="09152-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="09152-404">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="09152-404">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="09152-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="09152-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="09152-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="09152-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="09152-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="09152-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="09152-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="09152-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="09152-409">結構描述</span><span class="sxs-lookup"><span data-stu-id="09152-409">Schema</span></span>

<span data-ttu-id="09152-410">**IIS**</span><span class="sxs-lookup"><span data-stu-id="09152-410">**IIS**</span></span>

   * <span data-ttu-id="09152-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="09152-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="09152-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="09152-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="09152-413">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="09152-413">**IIS Express**</span></span>

   * <span data-ttu-id="09152-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="09152-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="09152-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="09152-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="09152-416">Configuration</span><span class="sxs-lookup"><span data-stu-id="09152-416">Configuration</span></span>

<span data-ttu-id="09152-417">**IIS**</span><span class="sxs-lookup"><span data-stu-id="09152-417">**IIS**</span></span>

   * <span data-ttu-id="09152-418">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="09152-418">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="09152-419">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="09152-419">**IIS Express**</span></span>

   * <span data-ttu-id="09152-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="09152-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="09152-421">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="09152-421">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>