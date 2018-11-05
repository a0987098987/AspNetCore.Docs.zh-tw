---
title: ASP.NET Core 模組設定參考
author: guardrex
description: 了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0d167f779f9dcae6b0d946dce5e341793daf43bf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50091011"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="b950c-103">ASP.NET Core 模組設定參考</span><span class="sxs-lookup"><span data-stu-id="b950c-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="b950c-104">作者：[Luke Latham](https://github.com/guardrex)[Rick Anderson](https://twitter.com/RickAndMSFT)及 [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="b950c-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="b950c-105">本文說明如何設定 ASP.NET Core 模組來裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b950c-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="b950c-106">如需 ASP.NET Core 模組簡介及安裝指示，請參閱 [ASP.NET Core 模組概觀](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="b950c-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="b950c-107">裝載模型</span><span class="sxs-lookup"><span data-stu-id="b950c-107">Hosting model</span></span>

<span data-ttu-id="b950c-108">針對執行 .NET Core 2.2 或更新版本的應用程式，該模組支援同處理序裝載模型，因此相較於反向 Proxy (跨處理序) 裝載會有更高的效能。</span><span class="sxs-lookup"><span data-stu-id="b950c-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="b950c-109">如需詳細資訊，請參閱<xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>。</span><span class="sxs-lookup"><span data-stu-id="b950c-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="b950c-110">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會針對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="b950c-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="b950c-111">若要設定同處理序裝載的應用程式，請將 `<AspNetCoreModuleHostingModel>` 屬性新增至應用程式的專案檔，其值為 `inprocess` (跨處理序裝載是使用 `outofprocess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="b950c-111">To configure an app for in-process hosting, add the `<AspNetCoreModuleHostingModel>` property to the app's project file with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreModuleHostingModel>inprocess</AspNetCoreModuleHostingModel>
</PropertyGroup>
```

<span data-ttu-id="b950c-112">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="b950c-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="b950c-113">不會使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="b950c-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="b950c-114">自訂 <xref:Microsoft.AspNetCore.Hosting.Server.IServer> 實作 `IISHttpServer` 可作為應用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="b950c-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="b950c-115">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="b950c-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="b950c-116">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="b950c-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="b950c-117">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="b950c-117">Use one app pool per app.</span></span>

* <span data-ttu-id="b950c-118">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="b950c-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="b950c-119">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="b950c-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="b950c-120">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="b950c-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="b950c-121">如果使用 `WebHostBuilder` (而不是使用 [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) 以手動方式設定應用程式的主機，而且曾在 Kestrel 伺服器上直接執行應用程式 (自我裝載)，請先呼叫 `UseKestrel`，再呼叫 `UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="b950c-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="b950c-122">如果順序相反，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="b950c-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="b950c-123">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="b950c-123">Client disconnects are detected.</span></span> <span data-ttu-id="b950c-124">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="b950c-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="b950c-125">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="b950c-125">Hosting model changes</span></span>

<span data-ttu-id="b950c-126">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-126">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="b950c-127">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="b950c-127">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="b950c-128">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-128">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="b950c-129">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="b950c-129">Process name</span></span>

<span data-ttu-id="b950c-130">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="b950c-130">`Process.GetCurrentProcess().ProcessName` reports either `w3wp` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="b950c-131">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="b950c-131">Configuration with web.config</span></span>

<span data-ttu-id="b950c-132">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="b950c-132">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="b950c-133">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="b950c-133">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
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

<span data-ttu-id="b950c-134">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="b950c-134">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" 
                hostingModel="inprocess" />
  </system.webServer>
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

<span data-ttu-id="b950c-135">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="b950c-135">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="b950c-136">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="b950c-136">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="b950c-137">如需子應用程式中 *web.config* 檔案設定相關的重要注意事項，請參閱[子應用程式設定](xref:host-and-deploy/iis/index#sub-application-configuration)。</span><span class="sxs-lookup"><span data-stu-id="b950c-137">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="b950c-138">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="b950c-138">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="b950c-139">屬性</span><span class="sxs-lookup"><span data-stu-id="b950c-139">Attribute</span></span> | <span data-ttu-id="b950c-140">描述</span><span class="sxs-lookup"><span data-stu-id="b950c-140">Description</span></span> | <span data-ttu-id="b950c-141">預設</span><span class="sxs-lookup"><span data-stu-id="b950c-141">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="b950c-142">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-142">Optional string attribute.</span></span></p><p><span data-ttu-id="b950c-143">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="b950c-143">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="b950c-144">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-144">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b950c-145">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="b950c-145">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="b950c-146">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-146">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b950c-147">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-147">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="b950c-148">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="b950c-148">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="b950c-149">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-149">Optional string attribute.</span></span></p><p><span data-ttu-id="b950c-150">將裝載模型指定為同處理序 (`inprocess`) 或跨處理序 (`outofprocess`)。</span><span class="sxs-lookup"><span data-stu-id="b950c-150">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="b950c-151">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-151">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-152">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="b950c-152">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="b950c-153">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="b950c-153">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="b950c-154">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="b950c-154">Default: `1`</span></span><br><span data-ttu-id="b950c-155">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="b950c-155">Min: `1`</span></span><br><span data-ttu-id="b950c-156">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="b950c-156">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="b950c-157">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-157">Required string attribute.</span></span></p><p><span data-ttu-id="b950c-158">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-158">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="b950c-159">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-159">Relative paths are supported.</span></span> <span data-ttu-id="b950c-160">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-160">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="b950c-161">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-161">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-162">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="b950c-162">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="b950c-163">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-163">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="b950c-164">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="b950c-164">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="b950c-165">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="b950c-165">Default: `10`</span></span><br><span data-ttu-id="b950c-166">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="b950c-166">Min: `0`</span></span><br><span data-ttu-id="b950c-167">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="b950c-167">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="b950c-168">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-168">Optional timespan attribute.</span></span></p><p><span data-ttu-id="b950c-169">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="b950c-169">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="b950c-170">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="b950c-170">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="b950c-171">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="b950c-171">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="b950c-172">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="b950c-172">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="b950c-173">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="b950c-173">Default: `00:02:00`</span></span><br><span data-ttu-id="b950c-174">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="b950c-174">Min: `00:00:00`</span></span><br><span data-ttu-id="b950c-175">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="b950c-175">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="b950c-176">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-176">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-177">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="b950c-177">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="b950c-178">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="b950c-178">Default: `10`</span></span><br><span data-ttu-id="b950c-179">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="b950c-179">Min: `0`</span></span><br><span data-ttu-id="b950c-180">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="b950c-180">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="b950c-181">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-182">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="b950c-182">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="b950c-183">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-183">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="b950c-184">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="b950c-184">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="b950c-185">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="b950c-185">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="b950c-186">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="b950c-186">Default: `120`</span></span><br><span data-ttu-id="b950c-187">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="b950c-187">Min: `0`</span></span><br><span data-ttu-id="b950c-188">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="b950c-188">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="b950c-189">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-189">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b950c-190">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="b950c-190">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="b950c-191">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-191">Optional string attribute.</span></span></p><p><span data-ttu-id="b950c-192">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-192">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="b950c-193">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="b950c-193">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="b950c-194">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-194">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="b950c-195">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b950c-195">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="b950c-196">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="b950c-196">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="b950c-197">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b950c-197">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="b950c-198">屬性</span><span class="sxs-lookup"><span data-stu-id="b950c-198">Attribute</span></span> | <span data-ttu-id="b950c-199">描述</span><span class="sxs-lookup"><span data-stu-id="b950c-199">Description</span></span> | <span data-ttu-id="b950c-200">預設</span><span class="sxs-lookup"><span data-stu-id="b950c-200">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="b950c-201">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-201">Optional string attribute.</span></span></p><p><span data-ttu-id="b950c-202">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="b950c-202">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="b950c-203">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-203">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b950c-204">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="b950c-204">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="b950c-205">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-205">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b950c-206">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-206">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="b950c-207">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="b950c-207">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="b950c-208">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-208">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-209">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="b950c-209">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="b950c-210">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="b950c-210">Default: `1`</span></span><br><span data-ttu-id="b950c-211">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="b950c-211">Min: `1`</span></span><br><span data-ttu-id="b950c-212">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="b950c-212">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="b950c-213">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-213">Required string attribute.</span></span></p><p><span data-ttu-id="b950c-214">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-214">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="b950c-215">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-215">Relative paths are supported.</span></span> <span data-ttu-id="b950c-216">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-216">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="b950c-217">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-217">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-218">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="b950c-218">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="b950c-219">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-219">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="b950c-220">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="b950c-220">Default: `10`</span></span><br><span data-ttu-id="b950c-221">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="b950c-221">Min: `0`</span></span><br><span data-ttu-id="b950c-222">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="b950c-222">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="b950c-223">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-223">Optional timespan attribute.</span></span></p><p><span data-ttu-id="b950c-224">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="b950c-224">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="b950c-225">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="b950c-225">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="b950c-226">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="b950c-226">Default: `00:02:00`</span></span><br><span data-ttu-id="b950c-227">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="b950c-227">Min: `00:00:00`</span></span><br><span data-ttu-id="b950c-228">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="b950c-228">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="b950c-229">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-229">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-230">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="b950c-230">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="b950c-231">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="b950c-231">Default: `10`</span></span><br><span data-ttu-id="b950c-232">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="b950c-232">Min: `0`</span></span><br><span data-ttu-id="b950c-233">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="b950c-233">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="b950c-234">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-235">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="b950c-235">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="b950c-236">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-236">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="b950c-237">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="b950c-237">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="b950c-238">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="b950c-238">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="b950c-239">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="b950c-239">Default: `120`</span></span><br><span data-ttu-id="b950c-240">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="b950c-240">Min: `0`</span></span><br><span data-ttu-id="b950c-241">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="b950c-241">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="b950c-242">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-242">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b950c-243">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="b950c-243">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="b950c-244">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-244">Optional string attribute.</span></span></p><p><span data-ttu-id="b950c-245">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-245">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="b950c-246">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="b950c-246">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="b950c-247">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-247">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="b950c-248">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b950c-248">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="b950c-249">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="b950c-249">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="b950c-250">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b950c-250">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="b950c-251">屬性</span><span class="sxs-lookup"><span data-stu-id="b950c-251">Attribute</span></span> | <span data-ttu-id="b950c-252">描述</span><span class="sxs-lookup"><span data-stu-id="b950c-252">Description</span></span> | <span data-ttu-id="b950c-253">預設</span><span class="sxs-lookup"><span data-stu-id="b950c-253">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="b950c-254">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-254">Optional string attribute.</span></span></p><p><span data-ttu-id="b950c-255">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="b950c-255">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="b950c-256">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-256">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b950c-257">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="b950c-257">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="b950c-258">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-258">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b950c-259">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-259">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="b950c-260">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="b950c-260">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="b950c-261">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-261">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-262">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="b950c-262">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="b950c-263">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="b950c-263">Default: `1`</span></span><br><span data-ttu-id="b950c-264">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="b950c-264">Min: `1`</span></span><br><span data-ttu-id="b950c-265">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="b950c-265">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="b950c-266">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-266">Required string attribute.</span></span></p><p><span data-ttu-id="b950c-267">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-267">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="b950c-268">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-268">Relative paths are supported.</span></span> <span data-ttu-id="b950c-269">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-269">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="b950c-270">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-270">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-271">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="b950c-271">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="b950c-272">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-272">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="b950c-273">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="b950c-273">Default: `10`</span></span><br><span data-ttu-id="b950c-274">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="b950c-274">Min: `0`</span></span><br><span data-ttu-id="b950c-275">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="b950c-275">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="b950c-276">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-276">Optional timespan attribute.</span></span></p><p><span data-ttu-id="b950c-277">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="b950c-277">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="b950c-278">在 ASP.NET Core 2.0 或更舊版本隨附的 ASP.NET Core 模組版本中，只能以整數分鐘為單位來指定 `requestTimeout`，否則會預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="b950c-278">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="b950c-279">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="b950c-279">Default: `00:02:00`</span></span><br><span data-ttu-id="b950c-280">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="b950c-280">Min: `00:00:00`</span></span><br><span data-ttu-id="b950c-281">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="b950c-281">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="b950c-282">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-282">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-283">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="b950c-283">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="b950c-284">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="b950c-284">Default: `10`</span></span><br><span data-ttu-id="b950c-285">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="b950c-285">Min: `0`</span></span><br><span data-ttu-id="b950c-286">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="b950c-286">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="b950c-287">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="b950c-288">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="b950c-288">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="b950c-289">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-289">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="b950c-290">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="b950c-290">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="b950c-291">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="b950c-291">Default: `120`</span></span><br><span data-ttu-id="b950c-292">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="b950c-292">Min: `0`</span></span><br><span data-ttu-id="b950c-293">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="b950c-293">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="b950c-294">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-294">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b950c-295">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="b950c-295">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="b950c-296">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-296">Optional string attribute.</span></span></p><p><span data-ttu-id="b950c-297">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-297">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="b950c-298">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="b950c-298">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="b950c-299">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-299">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="b950c-300">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b950c-300">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="b950c-301">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="b950c-301">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="b950c-302">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b950c-302">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="b950c-303">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="b950c-303">Setting environment variables</span></span>

<span data-ttu-id="b950c-304">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="b950c-304">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="b950c-305">請使用 `environmentVariables` 集合元素的 `environmentVariable` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="b950c-305">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="b950c-306">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="b950c-306">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="b950c-307">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="b950c-307">The following example sets two environment variables.</span></span> <span data-ttu-id="b950c-308">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="b950c-308">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="b950c-309">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="b950c-309">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="b950c-310">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-310">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="b950c-311">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="b950c-311">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="b950c-312">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="b950c-312">app_offline.htm</span></span>

<span data-ttu-id="b950c-313">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="b950c-313">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="b950c-314">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="b950c-314">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="b950c-315">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="b950c-315">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="b950c-316">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="b950c-316">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b950c-317">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="b950c-317">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="b950c-318">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="b950c-318">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="b950c-319">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="b950c-319">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b950c-320">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="b950c-320">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="b950c-321">如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="b950c-321">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="b950c-322">若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="b950c-322">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="b950c-323">如需有關設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤`<httpErrors>`](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="b950c-323">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="b950c-325">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="b950c-325">Log creation and redirection</span></span>

<span data-ttu-id="b950c-326">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="b950c-326">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="b950c-327">`stdoutLogFile` 路徑中的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b950c-327">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="b950c-328">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="b950c-328">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="b950c-329">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b950c-329">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="b950c-330">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="b950c-330">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="b950c-331">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b950c-331">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="b950c-332">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="b950c-332">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="b950c-333">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b950c-333">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b950c-334">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b950c-334">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="b950c-335">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="b950c-335">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="b950c-336">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 (*.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="b950c-336">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="b950c-337">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="b950c-337">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="b950c-338">下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="b950c-338">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="b950c-339">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="b950c-339">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="b950c-340">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b950c-340">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="b950c-341">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="b950c-341">Enhanced diagnostic logs</span></span>

<span data-ttu-id="b950c-342">ASP.NET Core 模組提供者是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="b950c-342">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="b950c-343">將 `<handlerSettings>` 項目新增至 *web.config* 中的 `<aspNetCore>` 項目。將 `debugLevel` 設定為 `TRACE` 會公開精確性更高的診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="b950c-343">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="b950c-344">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="b950c-344">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="b950c-345">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="b950c-345">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="b950c-346">ERROR</span><span class="sxs-lookup"><span data-stu-id="b950c-346">ERROR</span></span>
* <span data-ttu-id="b950c-347">WARNING</span><span class="sxs-lookup"><span data-stu-id="b950c-347">WARNING</span></span>
* <span data-ttu-id="b950c-348">INFO</span><span class="sxs-lookup"><span data-stu-id="b950c-348">INFO</span></span>
* <span data-ttu-id="b950c-349">TRACE</span><span class="sxs-lookup"><span data-stu-id="b950c-349">TRACE</span></span>

<span data-ttu-id="b950c-350">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="b950c-350">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="b950c-351">主控台</span><span class="sxs-lookup"><span data-stu-id="b950c-351">CONSOLE</span></span>
* <span data-ttu-id="b950c-352">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="b950c-352">EVENTLOG</span></span>
* <span data-ttu-id="b950c-353">檔案</span><span class="sxs-lookup"><span data-stu-id="b950c-353">FILE</span></span>

<span data-ttu-id="b950c-354">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="b950c-354">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="b950c-355">偵錯記錄檔的 `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; 路徑。</span><span class="sxs-lookup"><span data-stu-id="b950c-355">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="b950c-356">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="b950c-356">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="b950c-357">`ASPNETCORE_MODULE_DEBUG` &ndash; 偵錯層級設定。</span><span class="sxs-lookup"><span data-stu-id="b950c-357">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="b950c-358">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="b950c-358">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="b950c-359">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="b950c-359">The size of the log isn't limited.</span></span> <span data-ttu-id="b950c-360">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="b950c-360">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="b950c-361">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="b950c-361">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="b950c-362">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="b950c-362">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b950c-363">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="b950c-363">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="b950c-364">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="b950c-364">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="b950c-365">使用 HTTP 是一項效能最佳化作業，其中模組與 Kestrel 之間的流量會在網路介面外的回送位址進行。</span><span class="sxs-lookup"><span data-stu-id="b950c-365">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="b950c-366">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="b950c-366">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="b950c-367">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="b950c-367">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="b950c-368">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="b950c-368">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="b950c-369">配對權杖也會設定成每個代理要求的標頭 (`MSAspNetCoreToken`)。</span><span class="sxs-lookup"><span data-stu-id="b950c-369">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="b950c-370">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="b950c-370">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="b950c-371">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="b950c-371">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="b950c-372">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="b950c-372">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="b950c-373">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="b950c-373">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="b950c-374">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="b950c-374">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="b950c-375">ASP.NET Core 模組安裝程式會以 **SYSTEM** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="b950c-375">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="b950c-376">由於本機系統帳戶並不具備「IIS 共用設定」所使用共用路徑的修改權限，因此安裝程式在嘗試於共用上的 *applicationHost.config* 中進行模組設定時，會發生存取被拒錯誤。</span><span class="sxs-lookup"><span data-stu-id="b950c-376">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="b950c-377">使用「IIS 共用設定」時，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="b950c-377">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="b950c-378">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="b950c-378">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="b950c-379">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="b950c-379">Run the installer.</span></span>
1. <span data-ttu-id="b950c-380">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="b950c-380">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="b950c-381">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="b950c-381">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="b950c-382">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="b950c-382">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="b950c-383">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="b950c-383">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="b950c-384">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="b950c-384">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="b950c-385">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b950c-385">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="b950c-386">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="b950c-386">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="b950c-387">選取 [詳細資料] 索引標籤。[檔案版本] 和 [產品版本] 代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="b950c-387">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="b950c-388">模組的「裝載套件組合」安裝程式記錄檔位於 *C:\\Users\\%UserName%\\AppData\\Local\\Temp*。檔案的名稱為 *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*。</span><span class="sxs-lookup"><span data-stu-id="b950c-388">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="b950c-389">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="b950c-389">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="b950c-390">Module</span><span class="sxs-lookup"><span data-stu-id="b950c-390">Module</span></span>

<span data-ttu-id="b950c-391">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="b950c-391">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="b950c-392">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b950c-392">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="b950c-393">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b950c-393">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="b950c-394">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="b950c-394">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="b950c-395">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b950c-395">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="b950c-396">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b950c-396">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="b950c-397">結構描述</span><span class="sxs-lookup"><span data-stu-id="b950c-397">Schema</span></span>

<span data-ttu-id="b950c-398">**IIS**</span><span class="sxs-lookup"><span data-stu-id="b950c-398">**IIS**</span></span>

   * <span data-ttu-id="b950c-399">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="b950c-399">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="b950c-400">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="b950c-400">**IIS Express**</span></span>

   * <span data-ttu-id="b950c-401">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="b950c-401">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="b950c-402">Configuration</span><span class="sxs-lookup"><span data-stu-id="b950c-402">Configuration</span></span>

<span data-ttu-id="b950c-403">**IIS**</span><span class="sxs-lookup"><span data-stu-id="b950c-403">**IIS**</span></span>

   * <span data-ttu-id="b950c-404">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="b950c-404">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="b950c-405">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="b950c-405">**IIS Express**</span></span>

   * <span data-ttu-id="b950c-406">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="b950c-406">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="b950c-407">在 *applicationHost.config* 檔案中搜尋 *aspnetcore.dll*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="b950c-407">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="b950c-408">針對 IIS Express，*applicationHost.config* 檔案預設並不存在。</span><span class="sxs-lookup"><span data-stu-id="b950c-408">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="b950c-409">啟動 Visual Studio 方案中的任何 Web 應用程式專案時，便會在 *\<application_root>\\.vs\\config* 建立該檔案。</span><span class="sxs-lookup"><span data-stu-id="b950c-409">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
