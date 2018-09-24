---
title: ASP.NET Core 模組設定參考
author: guardrex
description: 了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: bf7a60b67b1ea78bb346e6dd5eeef38b54bfdbe4
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010945"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="cfdfd-103">ASP.NET Core 模組設定參考</span><span class="sxs-lookup"><span data-stu-id="cfdfd-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="cfdfd-104">作者：[Luke Latham](https://github.com/guardrex)[Rick Anderson](https://twitter.com/RickAndMSFT)及 [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="cfdfd-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="cfdfd-105">本文說明如何設定 ASP.NET Core 模組來裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="cfdfd-106">如需 ASP.NET Core 模組簡介及安裝指示，請參閱 [ASP.NET Core 模組概觀](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="cfdfd-107">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="cfdfd-107">Configuration with web.config</span></span>

<span data-ttu-id="cfdfd-108">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-108">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="cfdfd-109">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="cfdfd-109">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="cfdfd-110">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="cfdfd-110">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="cfdfd-111">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-111">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="cfdfd-112">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-112">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="cfdfd-113">如需子應用程式中 *web.config* 檔案設定相關的重要注意事項，請參閱[子應用程式設定](xref:host-and-deploy/iis/index#sub-application-configuration)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-113">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="cfdfd-114">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="cfdfd-114">Attributes of the aspNetCore element</span></span>

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="cfdfd-115">屬性</span><span class="sxs-lookup"><span data-stu-id="cfdfd-115">Attribute</span></span> | <span data-ttu-id="cfdfd-116">描述</span><span class="sxs-lookup"><span data-stu-id="cfdfd-116">Description</span></span> | <span data-ttu-id="cfdfd-117">預設</span><span class="sxs-lookup"><span data-stu-id="cfdfd-117">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="cfdfd-118">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-118">Optional string attribute.</span></span></p><p><span data-ttu-id="cfdfd-119">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-119">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <span data-ttu-id="cfdfd-120">true 或 false。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-120">true or false.</span></span></p><p><span data-ttu-id="cfdfd-121">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-121">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <span data-ttu-id="cfdfd-122">true 或 false。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-122">true or false.</span></span></p><p><span data-ttu-id="cfdfd-123">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-123">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="cfdfd-124">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-124">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processPath` | <p><span data-ttu-id="cfdfd-125">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-125">Required string attribute.</span></span></p><p><span data-ttu-id="cfdfd-126">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-126">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="cfdfd-127">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-127">Relative paths are supported.</span></span> <span data-ttu-id="cfdfd-128">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-128">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="cfdfd-129">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-129">Optional integer attribute.</span></span></p><p><span data-ttu-id="cfdfd-130">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-130">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="cfdfd-131">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-131">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | `10` |
| `requestTimeout` | <p><span data-ttu-id="cfdfd-132">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-132">Optional timespan attribute.</span></span></p><p><span data-ttu-id="cfdfd-133">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-133">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="cfdfd-134">在 ASP.NET Core 2.0 或更舊版本隨附的 ASP.NET Core 模組版本中，只能以整數分鐘為單位來指定 `requestTimeout`，否則會預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-134">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | `00:02:00` |
| `shutdownTimeLimit` | <p><span data-ttu-id="cfdfd-135">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-135">Optional integer attribute.</span></span></p><p><span data-ttu-id="cfdfd-136">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-136">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | `10` |
| `startupTimeLimit` | <p><span data-ttu-id="cfdfd-137">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="cfdfd-138">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-138">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="cfdfd-139">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-139">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="cfdfd-140">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-140">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | `120` |
| `stdoutLogEnabled` | <p><span data-ttu-id="cfdfd-141">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-141">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cfdfd-142">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-142">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="cfdfd-143">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-143">Optional string attribute.</span></span></p><p><span data-ttu-id="cfdfd-144">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-144">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="cfdfd-145">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-145">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="cfdfd-146">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-146">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="cfdfd-147">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-147">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="cfdfd-148">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-148">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="cfdfd-149">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-149">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="cfdfd-150">屬性</span><span class="sxs-lookup"><span data-stu-id="cfdfd-150">Attribute</span></span> | <span data-ttu-id="cfdfd-151">描述</span><span class="sxs-lookup"><span data-stu-id="cfdfd-151">Description</span></span> | <span data-ttu-id="cfdfd-152">預設</span><span class="sxs-lookup"><span data-stu-id="cfdfd-152">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="cfdfd-153">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-153">Optional string attribute.</span></span></p><p><span data-ttu-id="cfdfd-154">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-154">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <span data-ttu-id="cfdfd-155">true 或 false。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-155">true or false.</span></span></p><p><span data-ttu-id="cfdfd-156">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-156">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <span data-ttu-id="cfdfd-157">true 或 false。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-157">true or false.</span></span></p><p><span data-ttu-id="cfdfd-158">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-158">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="cfdfd-159">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-159">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processPath` | <p><span data-ttu-id="cfdfd-160">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-160">Required string attribute.</span></span></p><p><span data-ttu-id="cfdfd-161">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-161">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="cfdfd-162">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-162">Relative paths are supported.</span></span> <span data-ttu-id="cfdfd-163">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-163">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="cfdfd-164">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-164">Optional integer attribute.</span></span></p><p><span data-ttu-id="cfdfd-165">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-165">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="cfdfd-166">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-166">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | `10` |
| `requestTimeout` | <p><span data-ttu-id="cfdfd-167">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-167">Optional timespan attribute.</span></span></p><p><span data-ttu-id="cfdfd-168">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-168">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="cfdfd-169">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-169">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | `00:02:00` |
| `shutdownTimeLimit` | <p><span data-ttu-id="cfdfd-170">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-170">Optional integer attribute.</span></span></p><p><span data-ttu-id="cfdfd-171">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-171">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | `10` |
| `startupTimeLimit` | <p><span data-ttu-id="cfdfd-172">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-172">Optional integer attribute.</span></span></p><p><span data-ttu-id="cfdfd-173">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-173">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="cfdfd-174">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-174">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="cfdfd-175">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-175">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | `120` |
| `stdoutLogEnabled` | <p><span data-ttu-id="cfdfd-176">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-176">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="cfdfd-177">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-177">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="cfdfd-178">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-178">Optional string attribute.</span></span></p><p><span data-ttu-id="cfdfd-179">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-179">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="cfdfd-180">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-180">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="cfdfd-181">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-181">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="cfdfd-182">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-182">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="cfdfd-183">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-183">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="cfdfd-184">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-184">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="cfdfd-185">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="cfdfd-185">Setting environment variables</span></span>

<span data-ttu-id="cfdfd-186">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-186">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="cfdfd-187">請使用 `environmentVariables` 集合元素的 `environmentVariable` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-187">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="cfdfd-188">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-188">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="cfdfd-189">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-189">The following example sets two environment variables.</span></span> <span data-ttu-id="cfdfd-190">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-190">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="cfdfd-191">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-191">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="cfdfd-192">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-192">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="cfdfd-193">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-193">Only set the `ASPNETCORE_ENVIRONMENT` envirnonment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="cfdfd-194">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="cfdfd-194">app_offline.htm</span></span>

<span data-ttu-id="cfdfd-195">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-195">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="cfdfd-196">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-196">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="cfdfd-197">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-197">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="cfdfd-198">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-198">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="cfdfd-199">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="cfdfd-199">Start-up error page</span></span>

<span data-ttu-id="cfdfd-200">如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-200">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="cfdfd-201">若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-201">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="cfdfd-202">如需有關設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤`<httpErrors>`](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-202">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="cfdfd-204">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="cfdfd-204">Log creation and redirection</span></span>

<span data-ttu-id="cfdfd-205">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-205">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="cfdfd-206">`stdoutLogFile` 路徑中的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-206">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="cfdfd-207">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-207">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="cfdfd-208">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-208">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="cfdfd-209">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-209">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="cfdfd-210">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-210">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="cfdfd-211">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-211">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="cfdfd-212">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-212">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="cfdfd-213">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-213">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="cfdfd-214">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-214">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="cfdfd-215">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 (*.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-215">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="cfdfd-216">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-216">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="cfdfd-217">下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-217">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="cfdfd-218">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-218">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="cfdfd-219">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-219">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="cfdfd-220">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-220">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="cfdfd-221">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="cfdfd-221">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="cfdfd-222">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-222">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="cfdfd-223">使用 HTTP 是一項效能最佳化作業，其中模組與 Kestrel 之間的流量會在網路介面外的回送位址進行。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-223">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="cfdfd-224">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-224">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="cfdfd-225">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-225">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="cfdfd-226">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-226">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="cfdfd-227">配對權杖也會設定成每個代理要求的標頭 (`MSAspNetCoreToken`)。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-227">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="cfdfd-228">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-228">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="cfdfd-229">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-229">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="cfdfd-230">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-230">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="cfdfd-231">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-231">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="cfdfd-232">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="cfdfd-232">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="cfdfd-233">ASP.NET Core 模組安裝程式會以 **SYSTEM** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-233">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="cfdfd-234">由於本機系統帳戶並不具備「IIS 共用設定」所使用共用路徑的修改權限，因此安裝程式在嘗試於共用上的 *applicationHost.config* 中進行模組設定時，會發生存取被拒錯誤。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-234">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="cfdfd-235">使用「IIS 共用設定」時，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="cfdfd-235">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="cfdfd-236">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-236">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="cfdfd-237">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-237">Run the installer.</span></span>
1. <span data-ttu-id="cfdfd-238">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-238">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="cfdfd-239">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-239">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="cfdfd-240">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="cfdfd-240">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="cfdfd-241">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="cfdfd-241">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="cfdfd-242">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-242">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="cfdfd-243">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-243">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="cfdfd-244">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-244">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="cfdfd-245">選取 [詳細資料] 索引標籤。[檔案版本] 和 [產品版本] 代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-245">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="cfdfd-246">模組的「裝載套件組合」安裝程式記錄檔位於 *C:\\Users\\%UserName%\\AppData\\Local\\Temp*。檔案的名稱為 *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-246">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="cfdfd-247">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="cfdfd-247">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="cfdfd-248">Module</span><span class="sxs-lookup"><span data-stu-id="cfdfd-248">Module</span></span>

<span data-ttu-id="cfdfd-249">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="cfdfd-249">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="cfdfd-250">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cfdfd-250">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="cfdfd-251">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cfdfd-251">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="cfdfd-252">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="cfdfd-252">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="cfdfd-253">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cfdfd-253">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="cfdfd-254">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="cfdfd-254">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="cfdfd-255">結構描述</span><span class="sxs-lookup"><span data-stu-id="cfdfd-255">Schema</span></span>

<span data-ttu-id="cfdfd-256">**IIS**</span><span class="sxs-lookup"><span data-stu-id="cfdfd-256">**IIS**</span></span>

   * <span data-ttu-id="cfdfd-257">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="cfdfd-257">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="cfdfd-258">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cfdfd-258">**IIS Express**</span></span>

   * <span data-ttu-id="cfdfd-259">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="cfdfd-259">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="cfdfd-260">Configuration</span><span class="sxs-lookup"><span data-stu-id="cfdfd-260">Configuration</span></span>

<span data-ttu-id="cfdfd-261">**IIS**</span><span class="sxs-lookup"><span data-stu-id="cfdfd-261">**IIS**</span></span>

   * <span data-ttu-id="cfdfd-262">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="cfdfd-262">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="cfdfd-263">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="cfdfd-263">**IIS Express**</span></span>

   * <span data-ttu-id="cfdfd-264">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="cfdfd-264">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="cfdfd-265">在 *applicationHost.config* 檔案中搜尋 *aspnetcore.dll*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-265">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="cfdfd-266">針對 IIS Express，*applicationHost.config* 檔案預設並不存在。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-266">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="cfdfd-267">啟動 Visual Studio 方案中的任何 Web 應用程式專案時，便會在 *\<application_root>\\.vs\\config* 建立該檔案。</span><span class="sxs-lookup"><span data-stu-id="cfdfd-267">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
