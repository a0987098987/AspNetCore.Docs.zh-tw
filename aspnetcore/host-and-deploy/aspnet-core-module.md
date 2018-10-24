---
title: ASP.NET Core 模組設定參考
author: guardrex
description: 了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0ae19b26bc86c9da7a61f3117aaae1844115593a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913277"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="250d5-103">ASP.NET Core 模組設定參考</span><span class="sxs-lookup"><span data-stu-id="250d5-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="250d5-104">作者：[Luke Latham](https://github.com/guardrex)[Rick Anderson](https://twitter.com/RickAndMSFT)及 [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="250d5-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="250d5-105">本文說明如何設定 ASP.NET Core 模組來裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="250d5-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="250d5-106">如需 ASP.NET Core 模組簡介及安裝指示，請參閱 [ASP.NET Core 模組概觀](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="250d5-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="250d5-107">裝載模型</span><span class="sxs-lookup"><span data-stu-id="250d5-107">Hosting model</span></span>

<span data-ttu-id="250d5-108">針對執行 .NET Core 2.2 或更新版本的應用程式，該模組支援同處理序裝載模型，因此相較於反向 Proxy (跨處理序) 裝載會有更高的效能。</span><span class="sxs-lookup"><span data-stu-id="250d5-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="250d5-109">如需詳細資訊，請參閱<xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>。</span><span class="sxs-lookup"><span data-stu-id="250d5-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="250d5-110">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會針對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="250d5-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="250d5-111">若要設定同處理序裝載的應用程式，請將 `<AspNetCoreModuleHostingModel>` 屬性新增至應用程式的專案檔，其值為 `inprocess` (跨處理序裝載是使用 `outofprocess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="250d5-111">To configure an app for in-process hosting, add the `<AspNetCoreModuleHostingModel>` property to the app's project file with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreModuleHostingModel>inprocess</AspNetCoreModuleHostingModel>
</PropertyGroup>
```

<span data-ttu-id="250d5-112">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="250d5-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="250d5-113">不會使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="250d5-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="250d5-114">自訂 <xref:Microsoft.AspNetCore.Hosting.Server.IServer> 實作 `IISHttpServer` 可作為應用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="250d5-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="250d5-115">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="250d5-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="250d5-116">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="250d5-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="250d5-117">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="250d5-117">Use one app pool per app.</span></span>

* <span data-ttu-id="250d5-118">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="250d5-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="250d5-119">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="250d5-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="250d5-120">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="250d5-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="250d5-121">如果使用 `WebHostBuilder` (而不是使用 [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) 以手動方式設定應用程式的主機，而且曾在 Kestrel 伺服器上直接執行應用程式 (自我裝載)，請先呼叫 `UseKestrel`，再呼叫 `UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="250d5-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="250d5-122">如果順序相反，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="250d5-122">If the order is reversed, the host fails to start.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="250d5-123">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="250d5-123">Hosting model changes</span></span>

<span data-ttu-id="250d5-124">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-124">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="250d5-125">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="250d5-125">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="250d5-126">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-126">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="250d5-127">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="250d5-127">Process name</span></span>

<span data-ttu-id="250d5-128">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="250d5-128">`Process.GetCurrentProcess().ProcessName` reports either `w3wp` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="250d5-129">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="250d5-129">Configuration with web.config</span></span>

<span data-ttu-id="250d5-130">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="250d5-130">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="250d5-131">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="250d5-131">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="250d5-132">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="250d5-132">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="250d5-133">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="250d5-133">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="250d5-134">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="250d5-134">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="250d5-135">如需子應用程式中 *web.config* 檔案設定相關的重要注意事項，請參閱[子應用程式設定](xref:host-and-deploy/iis/index#sub-application-configuration)。</span><span class="sxs-lookup"><span data-stu-id="250d5-135">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="250d5-136">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="250d5-136">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="250d5-137">屬性</span><span class="sxs-lookup"><span data-stu-id="250d5-137">Attribute</span></span> | <span data-ttu-id="250d5-138">描述</span><span class="sxs-lookup"><span data-stu-id="250d5-138">Description</span></span> | <span data-ttu-id="250d5-139">預設</span><span class="sxs-lookup"><span data-stu-id="250d5-139">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="250d5-140">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-140">Optional string attribute.</span></span></p><p><span data-ttu-id="250d5-141">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="250d5-141">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="250d5-142">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-142">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="250d5-143">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="250d5-143">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="250d5-144">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-144">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="250d5-145">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-145">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="250d5-146">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="250d5-146">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="250d5-147">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-147">Optional string attribute.</span></span></p><p><span data-ttu-id="250d5-148">將裝載模型指定為同處理序 (`inprocess`) 或跨處理序 (`outofprocess`)。</span><span class="sxs-lookup"><span data-stu-id="250d5-148">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="250d5-149">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-149">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-150">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="250d5-150">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="250d5-151">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="250d5-151">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="250d5-152">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="250d5-152">Default: `1`</span></span><br><span data-ttu-id="250d5-153">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="250d5-153">Min: `1`</span></span><br><span data-ttu-id="250d5-154">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="250d5-154">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="250d5-155">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-155">Required string attribute.</span></span></p><p><span data-ttu-id="250d5-156">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-156">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="250d5-157">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-157">Relative paths are supported.</span></span> <span data-ttu-id="250d5-158">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-158">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="250d5-159">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-159">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-160">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="250d5-160">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="250d5-161">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-161">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="250d5-162">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="250d5-162">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="250d5-163">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="250d5-163">Default: `10`</span></span><br><span data-ttu-id="250d5-164">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="250d5-164">Min: `0`</span></span><br><span data-ttu-id="250d5-165">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="250d5-165">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="250d5-166">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-166">Optional timespan attribute.</span></span></p><p><span data-ttu-id="250d5-167">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="250d5-167">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="250d5-168">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="250d5-168">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="250d5-169">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="250d5-169">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="250d5-170">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="250d5-170">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="250d5-171">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="250d5-171">Default: `00:02:00`</span></span><br><span data-ttu-id="250d5-172">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="250d5-172">Min: `00:00:00`</span></span><br><span data-ttu-id="250d5-173">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="250d5-173">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="250d5-174">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-174">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-175">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="250d5-175">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="250d5-176">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="250d5-176">Default: `10`</span></span><br><span data-ttu-id="250d5-177">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="250d5-177">Min: `0`</span></span><br><span data-ttu-id="250d5-178">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="250d5-178">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="250d5-179">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-179">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-180">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="250d5-180">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="250d5-181">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-181">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="250d5-182">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="250d5-182">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="250d5-183">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="250d5-183">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="250d5-184">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="250d5-184">Default: `120`</span></span><br><span data-ttu-id="250d5-185">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="250d5-185">Min: `0`</span></span><br><span data-ttu-id="250d5-186">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="250d5-186">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="250d5-187">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-187">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="250d5-188">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="250d5-188">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="250d5-189">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-189">Optional string attribute.</span></span></p><p><span data-ttu-id="250d5-190">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-190">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="250d5-191">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="250d5-191">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="250d5-192">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-192">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="250d5-193">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="250d5-193">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="250d5-194">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="250d5-194">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="250d5-195">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="250d5-195">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="250d5-196">屬性</span><span class="sxs-lookup"><span data-stu-id="250d5-196">Attribute</span></span> | <span data-ttu-id="250d5-197">描述</span><span class="sxs-lookup"><span data-stu-id="250d5-197">Description</span></span> | <span data-ttu-id="250d5-198">預設</span><span class="sxs-lookup"><span data-stu-id="250d5-198">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="250d5-199">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-199">Optional string attribute.</span></span></p><p><span data-ttu-id="250d5-200">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="250d5-200">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="250d5-201">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-201">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="250d5-202">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="250d5-202">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="250d5-203">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-203">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="250d5-204">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-204">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="250d5-205">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="250d5-205">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="250d5-206">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-206">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-207">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="250d5-207">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="250d5-208">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="250d5-208">Default: `1`</span></span><br><span data-ttu-id="250d5-209">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="250d5-209">Min: `1`</span></span><br><span data-ttu-id="250d5-210">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="250d5-210">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="250d5-211">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-211">Required string attribute.</span></span></p><p><span data-ttu-id="250d5-212">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-212">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="250d5-213">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-213">Relative paths are supported.</span></span> <span data-ttu-id="250d5-214">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-214">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="250d5-215">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-215">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-216">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="250d5-216">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="250d5-217">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-217">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="250d5-218">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="250d5-218">Default: `10`</span></span><br><span data-ttu-id="250d5-219">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="250d5-219">Min: `0`</span></span><br><span data-ttu-id="250d5-220">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="250d5-220">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="250d5-221">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-221">Optional timespan attribute.</span></span></p><p><span data-ttu-id="250d5-222">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="250d5-222">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="250d5-223">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="250d5-223">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="250d5-224">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="250d5-224">Default: `00:02:00`</span></span><br><span data-ttu-id="250d5-225">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="250d5-225">Min: `00:00:00`</span></span><br><span data-ttu-id="250d5-226">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="250d5-226">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="250d5-227">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-227">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-228">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="250d5-228">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="250d5-229">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="250d5-229">Default: `10`</span></span><br><span data-ttu-id="250d5-230">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="250d5-230">Min: `0`</span></span><br><span data-ttu-id="250d5-231">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="250d5-231">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="250d5-232">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-232">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-233">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="250d5-233">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="250d5-234">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-234">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="250d5-235">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="250d5-235">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="250d5-236">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="250d5-236">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="250d5-237">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="250d5-237">Default: `120`</span></span><br><span data-ttu-id="250d5-238">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="250d5-238">Min: `0`</span></span><br><span data-ttu-id="250d5-239">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="250d5-239">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="250d5-240">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-240">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="250d5-241">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="250d5-241">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="250d5-242">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-242">Optional string attribute.</span></span></p><p><span data-ttu-id="250d5-243">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-243">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="250d5-244">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="250d5-244">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="250d5-245">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-245">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="250d5-246">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="250d5-246">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="250d5-247">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="250d5-247">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="250d5-248">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="250d5-248">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="250d5-249">屬性</span><span class="sxs-lookup"><span data-stu-id="250d5-249">Attribute</span></span> | <span data-ttu-id="250d5-250">描述</span><span class="sxs-lookup"><span data-stu-id="250d5-250">Description</span></span> | <span data-ttu-id="250d5-251">預設</span><span class="sxs-lookup"><span data-stu-id="250d5-251">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="250d5-252">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-252">Optional string attribute.</span></span></p><p><span data-ttu-id="250d5-253">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="250d5-253">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="250d5-254">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-254">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="250d5-255">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="250d5-255">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="250d5-256">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-256">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="250d5-257">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-257">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="250d5-258">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="250d5-258">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="250d5-259">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-259">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-260">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="250d5-260">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="250d5-261">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="250d5-261">Default: `1`</span></span><br><span data-ttu-id="250d5-262">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="250d5-262">Min: `1`</span></span><br><span data-ttu-id="250d5-263">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="250d5-263">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="250d5-264">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-264">Required string attribute.</span></span></p><p><span data-ttu-id="250d5-265">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-265">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="250d5-266">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-266">Relative paths are supported.</span></span> <span data-ttu-id="250d5-267">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-267">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="250d5-268">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-268">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-269">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="250d5-269">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="250d5-270">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-270">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="250d5-271">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="250d5-271">Default: `10`</span></span><br><span data-ttu-id="250d5-272">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="250d5-272">Min: `0`</span></span><br><span data-ttu-id="250d5-273">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="250d5-273">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="250d5-274">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-274">Optional timespan attribute.</span></span></p><p><span data-ttu-id="250d5-275">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="250d5-275">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="250d5-276">在 ASP.NET Core 2.0 或更舊版本隨附的 ASP.NET Core 模組版本中，只能以整數分鐘為單位來指定 `requestTimeout`，否則會預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="250d5-276">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="250d5-277">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="250d5-277">Default: `00:02:00`</span></span><br><span data-ttu-id="250d5-278">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="250d5-278">Min: `00:00:00`</span></span><br><span data-ttu-id="250d5-279">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="250d5-279">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="250d5-280">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-280">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-281">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="250d5-281">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="250d5-282">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="250d5-282">Default: `10`</span></span><br><span data-ttu-id="250d5-283">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="250d5-283">Min: `0`</span></span><br><span data-ttu-id="250d5-284">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="250d5-284">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="250d5-285">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-285">Optional integer attribute.</span></span></p><p><span data-ttu-id="250d5-286">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="250d5-286">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="250d5-287">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-287">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="250d5-288">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="250d5-288">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="250d5-289">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="250d5-289">Default: `120`</span></span><br><span data-ttu-id="250d5-290">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="250d5-290">Min: `0`</span></span><br><span data-ttu-id="250d5-291">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="250d5-291">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="250d5-292">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-292">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="250d5-293">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="250d5-293">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="250d5-294">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-294">Optional string attribute.</span></span></p><p><span data-ttu-id="250d5-295">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-295">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="250d5-296">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="250d5-296">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="250d5-297">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-297">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="250d5-298">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="250d5-298">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="250d5-299">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="250d5-299">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="250d5-300">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="250d5-300">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="250d5-301">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="250d5-301">Setting environment variables</span></span>

<span data-ttu-id="250d5-302">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="250d5-302">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="250d5-303">請使用 `environmentVariables` 集合元素的 `environmentVariable` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="250d5-303">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="250d5-304">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="250d5-304">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="250d5-305">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="250d5-305">The following example sets two environment variables.</span></span> <span data-ttu-id="250d5-306">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="250d5-306">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="250d5-307">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="250d5-307">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="250d5-308">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="250d5-308">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="250d5-309">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="250d5-309">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="250d5-310">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="250d5-310">app_offline.htm</span></span>

<span data-ttu-id="250d5-311">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="250d5-311">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="250d5-312">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="250d5-312">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="250d5-313">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="250d5-313">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="250d5-314">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="250d5-314">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="250d5-315">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="250d5-315">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="250d5-316">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="250d5-316">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="250d5-317">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="250d5-317">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="250d5-318">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="250d5-318">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="250d5-319">如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="250d5-319">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="250d5-320">若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="250d5-320">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="250d5-321">如需有關設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤`<httpErrors>`](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="250d5-321">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="250d5-323">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="250d5-323">Log creation and redirection</span></span>

<span data-ttu-id="250d5-324">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="250d5-324">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="250d5-325">`stdoutLogFile` 路徑中的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="250d5-325">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="250d5-326">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="250d5-326">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="250d5-327">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="250d5-327">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="250d5-328">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="250d5-328">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="250d5-329">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="250d5-329">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="250d5-330">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="250d5-330">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="250d5-331">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="250d5-331">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="250d5-332">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="250d5-332">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="250d5-333">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="250d5-333">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="250d5-334">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 (*.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="250d5-334">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="250d5-335">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="250d5-335">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="250d5-336">下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="250d5-336">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="250d5-337">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="250d5-337">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="250d5-338">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="250d5-338">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

<span data-ttu-id="250d5-339">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="250d5-339">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="250d5-340">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="250d5-340">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="250d5-341">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="250d5-341">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="250d5-342">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="250d5-342">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="250d5-343">使用 HTTP 是一項效能最佳化作業，其中模組與 Kestrel 之間的流量會在網路介面外的回送位址進行。</span><span class="sxs-lookup"><span data-stu-id="250d5-343">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="250d5-344">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="250d5-344">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="250d5-345">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="250d5-345">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="250d5-346">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="250d5-346">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="250d5-347">配對權杖也會設定成每個代理要求的標頭 (`MSAspNetCoreToken`)。</span><span class="sxs-lookup"><span data-stu-id="250d5-347">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="250d5-348">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="250d5-348">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="250d5-349">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="250d5-349">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="250d5-350">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="250d5-350">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="250d5-351">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="250d5-351">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="250d5-352">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="250d5-352">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="250d5-353">ASP.NET Core 模組安裝程式會以 **SYSTEM** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="250d5-353">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="250d5-354">由於本機系統帳戶並不具備「IIS 共用設定」所使用共用路徑的修改權限，因此安裝程式在嘗試於共用上的 *applicationHost.config* 中進行模組設定時，會發生存取被拒錯誤。</span><span class="sxs-lookup"><span data-stu-id="250d5-354">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="250d5-355">使用「IIS 共用設定」時，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="250d5-355">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="250d5-356">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="250d5-356">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="250d5-357">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="250d5-357">Run the installer.</span></span>
1. <span data-ttu-id="250d5-358">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="250d5-358">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="250d5-359">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="250d5-359">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="250d5-360">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="250d5-360">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="250d5-361">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="250d5-361">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="250d5-362">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="250d5-362">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="250d5-363">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="250d5-363">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="250d5-364">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="250d5-364">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="250d5-365">選取 [詳細資料] 索引標籤。[檔案版本] 和 [產品版本] 代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="250d5-365">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="250d5-366">模組的「裝載套件組合」安裝程式記錄檔位於 *C:\\Users\\%UserName%\\AppData\\Local\\Temp*。檔案的名稱為 *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*。</span><span class="sxs-lookup"><span data-stu-id="250d5-366">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="250d5-367">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="250d5-367">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="250d5-368">Module</span><span class="sxs-lookup"><span data-stu-id="250d5-368">Module</span></span>

<span data-ttu-id="250d5-369">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="250d5-369">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="250d5-370">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="250d5-370">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="250d5-371">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="250d5-371">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="250d5-372">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="250d5-372">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="250d5-373">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="250d5-373">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="250d5-374">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="250d5-374">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="250d5-375">結構描述</span><span class="sxs-lookup"><span data-stu-id="250d5-375">Schema</span></span>

<span data-ttu-id="250d5-376">**IIS**</span><span class="sxs-lookup"><span data-stu-id="250d5-376">**IIS**</span></span>

   * <span data-ttu-id="250d5-377">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="250d5-377">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="250d5-378">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="250d5-378">**IIS Express**</span></span>

   * <span data-ttu-id="250d5-379">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="250d5-379">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="250d5-380">Configuration</span><span class="sxs-lookup"><span data-stu-id="250d5-380">Configuration</span></span>

<span data-ttu-id="250d5-381">**IIS**</span><span class="sxs-lookup"><span data-stu-id="250d5-381">**IIS**</span></span>

   * <span data-ttu-id="250d5-382">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="250d5-382">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="250d5-383">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="250d5-383">**IIS Express**</span></span>

   * <span data-ttu-id="250d5-384">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="250d5-384">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="250d5-385">在 *applicationHost.config* 檔案中搜尋 *aspnetcore.dll*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="250d5-385">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="250d5-386">針對 IIS Express，*applicationHost.config* 檔案預設並不存在。</span><span class="sxs-lookup"><span data-stu-id="250d5-386">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="250d5-387">啟動 Visual Studio 方案中的任何 Web 應用程式專案時，便會在 *\<application_root>\\.vs\\config* 建立該檔案。</span><span class="sxs-lookup"><span data-stu-id="250d5-387">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
