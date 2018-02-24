---
title: "ASP.NET 核心模組的組態參考"
author: guardrex
description: "了解如何設定適用於主控 ASP.NET Core 應用程式的 ASP.NET 核心模組。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c01abed767a226eae68725c1c53d922eac2f705e
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="7bb38-103">ASP.NET 核心模組的組態參考</span><span class="sxs-lookup"><span data-stu-id="7bb38-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="7bb38-104">由[Luke Latham](https://github.com/guardrex)， [Rick Anderson](https://twitter.com/RickAndMSFT)，和[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="7bb38-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="7bb38-105">本文件提供有關如何設定適用於主控 ASP.NET Core 應用程式的 ASP.NET 核心模組的指示。</span><span class="sxs-lookup"><span data-stu-id="7bb38-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="7bb38-106">如需 ASP.NET Core 模組和安裝指示的簡介，請參閱[ASP.NET Core 模組概觀](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="7bb38-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="7bb38-107">Web.config 組態</span><span class="sxs-lookup"><span data-stu-id="7bb38-107">Configuration with web.config</span></span>

<span data-ttu-id="7bb38-108">設定 ASP.NET 核心模組`aspNetCore`區段`system.webServer`中站台的節點*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="7bb38-108">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="7bb38-109">下列*web.config*發行檔案以進行[framework 相依的部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)和設定 ASP.NET 核心模組，以處理站台的要求：</span><span class="sxs-lookup"><span data-stu-id="7bb38-109">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="7bb38-110">下列*web.config*發行以供[獨立的部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="7bb38-110">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="7bb38-111">當應用程式部署至[Azure App Service](https://azure.microsoft.com/services/app-service/)、`stdoutLogFile`路徑會設定為`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="7bb38-111">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="7bb38-112">路徑會將 stdout 記錄檔，以儲存*LogFiles*資料夾中，這是放置位置自動服務所建立的。</span><span class="sxs-lookup"><span data-stu-id="7bb38-112">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="7bb38-113">請參閱[子應用程式組態](xref:host-and-deploy/iis/index#sub-application-configuration)的組態相關的重要注意事項*web.config*子應用程式中的檔案。</span><span class="sxs-lookup"><span data-stu-id="7bb38-113">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="7bb38-114">AspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="7bb38-114">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="7bb38-115">屬性</span><span class="sxs-lookup"><span data-stu-id="7bb38-115">Attribute</span></span> | <span data-ttu-id="7bb38-116">描述</span><span class="sxs-lookup"><span data-stu-id="7bb38-116">Description</span></span> | <span data-ttu-id="7bb38-117">預設</span><span class="sxs-lookup"><span data-stu-id="7bb38-117">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="7bb38-118">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="7bb38-118">Optional string attribute.</span></span></p><p><span data-ttu-id="7bb38-119">可執行檔中指定的引數**processPath**。</span><span class="sxs-lookup"><span data-stu-id="7bb38-119">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <span data-ttu-id="7bb38-120">true 或 false。</span><span class="sxs-lookup"><span data-stu-id="7bb38-120">true or false.</span></span></p><p><span data-ttu-id="7bb38-121">如果為 true， **502.5-處理序失敗**隱藏頁面時，且狀態 502 字碼頁設定*web.config*優先。</span><span class="sxs-lookup"><span data-stu-id="7bb38-121">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <span data-ttu-id="7bb38-122">true 或 false。</span><span class="sxs-lookup"><span data-stu-id="7bb38-122">true or false.</span></span></p><p><span data-ttu-id="7bb38-123">如果為 true，則會將權杖轉送到接聽 %aspnetcore_port%做為每個要求的標頭 ' MS-ASPNETCORE-WINAUTHTOKEN' 的子處理序。</span><span class="sxs-lookup"><span data-stu-id="7bb38-123">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="7bb38-124">它負責這個語彙基元，每個要求上呼叫 CloseHandle 該程序。</span><span class="sxs-lookup"><span data-stu-id="7bb38-124">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processPath` | <p><span data-ttu-id="7bb38-125">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="7bb38-125">Required string attribute.</span></span></p><p><span data-ttu-id="7bb38-126">啟動接聽 HTTP 要求的處理序的可執行檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="7bb38-126">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="7bb38-127">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="7bb38-127">Relative paths are supported.</span></span> <span data-ttu-id="7bb38-128">如果路徑是以開頭`.`，路徑會被視為相對於網站根目錄。</span><span class="sxs-lookup"><span data-stu-id="7bb38-128">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="7bb38-129">選擇性整數屬性。</span><span class="sxs-lookup"><span data-stu-id="7bb38-129">Optional integer attribute.</span></span></p><p><span data-ttu-id="7bb38-130">指定處理程序中指定的次數**processPath**允許每分鐘的損毀。</span><span class="sxs-lookup"><span data-stu-id="7bb38-130">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="7bb38-131">如果超過此限制時，模組會停止啟動該分鐘的剩餘的處理程序。</span><span class="sxs-lookup"><span data-stu-id="7bb38-131">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | `10` |
| `requestTimeout` | <p><span data-ttu-id="7bb38-132">選擇性 timespan 屬性。</span><span class="sxs-lookup"><span data-stu-id="7bb38-132">Optional timespan attribute.</span></span></p><p><span data-ttu-id="7bb38-133">指定 ASP.NET 核心模組等候來自接聽 %aspnetcore_port%之處理序的回應持續期間。</span><span class="sxs-lookup"><span data-stu-id="7bb38-133">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="7bb38-134">`requestTimeout`必須指定以分鐘為單位，否則它會預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="7bb38-134">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | `00:02:00` |
| `shutdownTimeLimit` | <p><span data-ttu-id="7bb38-135">選擇性整數屬性。</span><span class="sxs-lookup"><span data-stu-id="7bb38-135">Optional integer attribute.</span></span></p><p><span data-ttu-id="7bb38-136">持續時間，以秒為單位的模組，可依正常程序關閉時*app_offline.htm*偵測到的檔案。</span><span class="sxs-lookup"><span data-stu-id="7bb38-136">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | `10` |
| `startupTimeLimit` | <p><span data-ttu-id="7bb38-137">選擇性整數屬性。</span><span class="sxs-lookup"><span data-stu-id="7bb38-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="7bb38-138">以秒為單位的模組，可開始接聽連接埠之處理序的持續時間。</span><span class="sxs-lookup"><span data-stu-id="7bb38-138">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="7bb38-139">如果超過此時間限制，此模組會清除程序。</span><span class="sxs-lookup"><span data-stu-id="7bb38-139">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="7bb38-140">模組會嘗試重新啟動處理程序，當它接收新要求，並會繼續嘗試重新啟動的程序，在後續的內送要求，除非應用程式無法啟動**rapidFailsPerMinute**在上一次循環的分鐘。</span><span class="sxs-lookup"><span data-stu-id="7bb38-140">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | `120` |
| `stdoutLogEnabled` | <p><span data-ttu-id="7bb38-141">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="7bb38-141">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="7bb38-142">如果為 true， **stdout**和**stderr**中指定的處理序**processPath**會重新導向至指定的檔案**stdoutLogFile**。</span><span class="sxs-lookup"><span data-stu-id="7bb38-142">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="7bb38-143">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="7bb38-143">Optional string attribute.</span></span></p><p><span data-ttu-id="7bb38-144">指定的相對或絕對檔案路徑的**stdout**和**stderr**中指定的處理序從**processPath**記錄。</span><span class="sxs-lookup"><span data-stu-id="7bb38-144">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="7bb38-145">相對路徑會相對於網站根目錄。</span><span class="sxs-lookup"><span data-stu-id="7bb38-145">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="7bb38-146">從任何路徑`.`是相對於網站根目錄和所有其他路徑會被視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="7bb38-146">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="7bb38-147">提供的路徑中任何資料夾必須存在於要建立的記錄檔之模組的順序。</span><span class="sxs-lookup"><span data-stu-id="7bb38-147">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="7bb38-148">使用底線分隔符號、 時間戳記、 處理序識別碼和副檔名 (*.log*) 會加入至最後一個區段**stdoutLogFile**路徑。</span><span class="sxs-lookup"><span data-stu-id="7bb38-148">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="7bb38-149">如果`.\logs\stdout`提供為數值，而範例 stdout 記錄會儲存為*stdout_20180205194132_1934.log*中*記錄*資料夾時儲存在 2/5/2018年 19:41:32 與 1934年的處理序識別碼。</span><span class="sxs-lookup"><span data-stu-id="7bb38-149">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="7bb38-150">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="7bb38-150">Setting environment variables</span></span>

<span data-ttu-id="7bb38-151">中的程序可以指定環境變數`processPath`屬性。</span><span class="sxs-lookup"><span data-stu-id="7bb38-151">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="7bb38-152">指定的環境變數`environmentVariable`子項目`environmentVariables`集合項目。</span><span class="sxs-lookup"><span data-stu-id="7bb38-152">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="7bb38-153">此區段中設定的環境變數優先於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="7bb38-153">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="7bb38-154">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="7bb38-154">The following example sets two environment variables.</span></span> <span data-ttu-id="7bb38-155">`ASPNETCORE_ENVIRONMENT` 設定應用程式的環境`Development`。</span><span class="sxs-lookup"><span data-stu-id="7bb38-155">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="7bb38-156">開發人員可能會暫時將此值設定*web.config*檔案，以便強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入偵錯應用程式例外狀況時。</span><span class="sxs-lookup"><span data-stu-id="7bb38-156">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="7bb38-157">`CONFIG_DIR` 是使用者定義的環境變數的範例，開發人員已寫入讀取的值，在啟動時形成一個載入的應用程式組態檔路徑的程式碼的位置。</span><span class="sxs-lookup"><span data-stu-id="7bb38-157">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="7bb38-158">只能設定`ASPNETCORE_ENVIRONMENT`envirnonment 變數`Development`籌畫與測試無法存取不受信任的網路，例如網際網路的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="7bb38-158">Only set the `ASPNETCORE_ENVIRONMENT` envirnonment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="7bb38-159">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="7bb38-159">app_offline.htm</span></span>

<span data-ttu-id="7bb38-160">如果同名的檔案*app_offline.htm*偵測到應用程式的根目錄中 ASP.NET 核心模組嘗試依正常程序關閉應用程式，然後停止處理內送要求。</span><span class="sxs-lookup"><span data-stu-id="7bb38-160">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="7bb38-161">如果應用程式仍在執行中定義的秒數後`shutdownTimeLimit`，ASP.NET 核心模組會清除執行的處理序。</span><span class="sxs-lookup"><span data-stu-id="7bb38-161">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="7bb38-162">雖然*app_offline.htm*檔案是否存在，ASP.NET 核心模組回應要求傳送回內容*app_offline.htm*檔案。</span><span class="sxs-lookup"><span data-stu-id="7bb38-162">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="7bb38-163">當*app_offline.htm*會移除檔案，在下一個要求會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="7bb38-163">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="7bb38-164">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="7bb38-164">Start-up error page</span></span>

<span data-ttu-id="7bb38-165">如果 ASP.NET 核心模組無法啟動後端程序或後端程序會啟動但無法在設定的連接埠上接聽*502.5 的處理序失敗*狀態程式碼 頁面隨即出現。</span><span class="sxs-lookup"><span data-stu-id="7bb38-165">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="7bb38-166">若要隱藏這個頁面，並還原至預設 IIS 502 狀態字碼頁，請使用`disableStartUpErrorPage`屬性。</span><span class="sxs-lookup"><span data-stu-id="7bb38-166">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="7bb38-167">如需有關如何設定自訂錯誤訊息的詳細資訊，請參閱[HTTP 錯誤`<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="7bb38-167">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 程序失敗狀態的字碼頁](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="7bb38-169">記錄檔的建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="7bb38-169">Log creation and redirection</span></span>

<span data-ttu-id="7bb38-170">ASP.NET 核心模組重新導向`stdout`和`stderr`磁碟的記錄檔`stdoutLogEnabled`和`stdoutLogFile`屬性`aspNetCore`設定項目。</span><span class="sxs-lookup"><span data-stu-id="7bb38-170">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="7bb38-171">中的任何資料夾`stdoutLogFile`路徑必須存在於要建立的記錄檔之模組的順序。</span><span class="sxs-lookup"><span data-stu-id="7bb38-171">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="7bb38-172">建立記錄檔時，會自動加入時間戳記與檔案的副檔名。</span><span class="sxs-lookup"><span data-stu-id="7bb38-172">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="7bb38-173">不旋轉記錄檔，除非處理序回收/重新啟動，就會發生。</span><span class="sxs-lookup"><span data-stu-id="7bb38-173">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="7bb38-174">它是主機服務提供者來限制記錄檔使用的磁碟空間的責任。</span><span class="sxs-lookup"><span data-stu-id="7bb38-174">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="7bb38-175">使用`stdout`記錄只建議用於疑難排解應用程式啟動問題。</span><span class="sxs-lookup"><span data-stu-id="7bb38-175">Using the `stdout` log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="7bb38-176">一般應用程式記錄檔，請勿使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7bb38-176">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="7bb38-177">例行記錄中的 ASP.NET Core 應用程式、 使用限制記錄檔大小，並且會旋轉記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7bb38-177">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7bb38-178">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7bb38-178">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="7bb38-179">記錄檔名稱由附加的時間戳記、 處理序識別碼和副檔名 (*.log*) 的最後一個區段以`stdoutLogFile`路徑 (通常*stdout*) 底線字元分隔。</span><span class="sxs-lookup"><span data-stu-id="7bb38-179">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="7bb38-180">如果`stdoutLogFile`路徑的結尾*stdout*，記錄檔的應用程式，但在 2/5/2018年端建立 19:42:32 1934 PID 有檔案名稱*stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="7bb38-180">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="7bb38-181">下列範例`aspNetCore`項目會設定`stdout`Azure App Service 中裝載的應用程式的記錄功能。</span><span class="sxs-lookup"><span data-stu-id="7bb38-181">The following sample `aspNetCore` element configures `stdout` logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="7bb38-182">本機路徑或網路共用路徑是可接受的本機記錄。</span><span class="sxs-lookup"><span data-stu-id="7bb38-182">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="7bb38-183">確認應用程式集區使用者識別必須提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="7bb38-183">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="7bb38-184">請參閱[web.config 組態](#configuration-with-webconfig)的範例，`aspNetCore`中的項目*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="7bb38-184">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="7bb38-185">IIS 與 ASP.NET Core 模組共用設定</span><span class="sxs-lookup"><span data-stu-id="7bb38-185">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="7bb38-186">ASP.NET 核心模組安裝程式會執行與的權限**系統**帳戶。</span><span class="sxs-lookup"><span data-stu-id="7bb38-186">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="7bb38-187">安裝程式的本機系統帳戶不會有修改 IIS 共用設定所使用的共用路徑的權限，因為遇到拒絕存取錯誤時嘗試設定中的模組設定*applicationHost.config*共用上。</span><span class="sxs-lookup"><span data-stu-id="7bb38-187">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="7bb38-188">當使用 IIS 共用設定，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="7bb38-188">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="7bb38-189">停用 IIS 共用的設定。</span><span class="sxs-lookup"><span data-stu-id="7bb38-189">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="7bb38-190">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="7bb38-190">Run the installer.</span></span>
1. <span data-ttu-id="7bb38-191">匯出已更新*applicationHost.config*共用的檔案。</span><span class="sxs-lookup"><span data-stu-id="7bb38-191">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="7bb38-192">重新啟用 IIS 共用的設定。</span><span class="sxs-lookup"><span data-stu-id="7bb38-192">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="7bb38-193">模組版本，以及裝載配套安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="7bb38-193">Module version and hosting bundle installer logs</span></span>

<span data-ttu-id="7bb38-194">若要判斷已安裝的 ASP.NET 核心模組版本：</span><span class="sxs-lookup"><span data-stu-id="7bb38-194">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="7bb38-195">在主機系統上，瀏覽至*%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="7bb38-195">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="7bb38-196">找出*aspnetcore.dll*檔案。</span><span class="sxs-lookup"><span data-stu-id="7bb38-196">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="7bb38-197">以滑鼠右鍵按一下檔案，然後選取**屬性**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="7bb38-197">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="7bb38-198">選取**詳細資料** 索引標籤。**檔案版本**和**產品版本**代表已安裝之模組的版本。</span><span class="sxs-lookup"><span data-stu-id="7bb38-198">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="7bb38-199">模組的 Windows 伺服器裝載配套安裝程式記錄檔會位於*c:\\使用者\\%username%\\AppData\\本機\\Temp*。檔案命名為*dd_DotNetCoreWinSvrHosting__\<時間戳記 > _000_AspNetCoreModule_x64.log*。</span><span class="sxs-lookup"><span data-stu-id="7bb38-199">The Windows Server Hosting bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="7bb38-200">模組、 結構描述和組態檔的位置</span><span class="sxs-lookup"><span data-stu-id="7bb38-200">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="7bb38-201">Module</span><span class="sxs-lookup"><span data-stu-id="7bb38-201">Module</span></span>

<span data-ttu-id="7bb38-202">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="7bb38-202">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="7bb38-203">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="7bb38-203">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="7bb38-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="7bb38-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="7bb38-205">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="7bb38-205">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="7bb38-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="7bb38-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="7bb38-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="7bb38-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="7bb38-208">結構描述</span><span class="sxs-lookup"><span data-stu-id="7bb38-208">Schema</span></span>

<span data-ttu-id="7bb38-209">**IIS**</span><span class="sxs-lookup"><span data-stu-id="7bb38-209">**IIS**</span></span>

   * <span data-ttu-id="7bb38-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="7bb38-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="7bb38-211">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="7bb38-211">**IIS Express**</span></span>

   * <span data-ttu-id="7bb38-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="7bb38-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="7bb38-213">組態</span><span class="sxs-lookup"><span data-stu-id="7bb38-213">Configuration</span></span>

<span data-ttu-id="7bb38-214">**IIS**</span><span class="sxs-lookup"><span data-stu-id="7bb38-214">**IIS**</span></span>

   * <span data-ttu-id="7bb38-215">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="7bb38-215">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="7bb38-216">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="7bb38-216">**IIS Express**</span></span>

   * <span data-ttu-id="7bb38-217">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="7bb38-217">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="7bb38-218">可以找到檔案，藉由搜尋*aspnetcore.dll*中*applicationHost.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="7bb38-218">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="7bb38-219">IIS express， *applicationHost.config*根據預設，檔案不存在。</span><span class="sxs-lookup"><span data-stu-id="7bb38-219">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="7bb38-220">檔案建立在 *\<application_root >\\.vs\\config*時啟動 Visual Studio 方案中的任何 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="7bb38-220">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
