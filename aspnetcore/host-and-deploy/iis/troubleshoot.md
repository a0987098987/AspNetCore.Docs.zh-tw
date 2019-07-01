---
title: 針對 IIS 上的 ASP.NET Core 進行疑難排解
author: guardrex
description: 了解如何診斷 ASP.NET Core 應用程式的 Internet Information Services (IIS) 部署問題。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 4df370dd9b1a5a651bcf767b8b9ace4220bdc345
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313649"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="60b0c-103">針對 IIS 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="60b0c-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="60b0c-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="60b0c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="60b0c-105">本文說明以 [Internet Information Services (IIS)](/iis) 裝載 ASP.NET Core 應用程式時，如何診斷此應用程式的啟動問題。</span><span class="sxs-lookup"><span data-stu-id="60b0c-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="60b0c-106">本文中資訊適用的裝載環境是 Windows Server 和 Windows 桌面上的 IIS。</span><span class="sxs-lookup"><span data-stu-id="60b0c-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

<span data-ttu-id="60b0c-107">其他疑難排解主題：</span><span class="sxs-lookup"><span data-stu-id="60b0c-107">Additional troubleshooting topics:</span></span>

* <span data-ttu-id="60b0c-108">Azure App Service 也會使用 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) 與 IIS 來裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="60b0c-108">Azure App Service also uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps.</span></span> <span data-ttu-id="60b0c-109">如需有關 Azure App Service 的疑難排解資訊，請參閱 <xref:host-and-deploy/azure-apps/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="60b0c-109">For troubleshooting advice that pertains specifically to Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
* <span data-ttu-id="60b0c-110"><xref:fundamentals/error-handling> 涵蓋在本機系統上進行開發時，如何處理 ASP.NET Core 應用程式中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="60b0c-110"><xref:fundamentals/error-handling> covers how to handle errors in ASP.NET Core apps during development on a local system.</span></span>
* <span data-ttu-id="60b0c-111">[了解如何使用 Visual Studio 進行偵錯](/visualstudio/debugger/getting-started-with-the-debugger)介紹 Visual Studio 偵錯工具的功能。</span><span class="sxs-lookup"><span data-stu-id="60b0c-111">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) introduces the features of the Visual Studio debugger.</span></span>
* <span data-ttu-id="60b0c-112">[使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\) 說明 Visual Studio Code 中內建的偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="60b0c-112">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) describes the debugging support built into Visual Studio Code.</span></span>

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="60b0c-113">針對應用程式啟動錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="60b0c-113">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="60b0c-114">啟用 ASP.NET Core 模組偵錯記錄</span><span class="sxs-lookup"><span data-stu-id="60b0c-114">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="60b0c-115">將下列處理常式設定新增至應用程式的 *web.config* 檔案，以啟用 ASP.NET Core 模組偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="60b0c-115">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="60b0c-116">確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="60b0c-116">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="60b0c-117">應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="60b0c-117">Application Event Log</span></span>

<span data-ttu-id="60b0c-118">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="60b0c-118">Access the Application Event Log:</span></span>

1. <span data-ttu-id="60b0c-119">開啟 [開始] 功能表，搜尋**事件檢視器**，然後選取 [事件檢視器]  應用程式。</span><span class="sxs-lookup"><span data-stu-id="60b0c-119">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="60b0c-120">在 [事件檢視器]  中，開啟 [Windows 記錄]  節點。</span><span class="sxs-lookup"><span data-stu-id="60b0c-120">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="60b0c-121">選取 [應用程式]  以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="60b0c-121">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="60b0c-122">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="60b0c-122">Search for errors associated with the failing app.</span></span> <span data-ttu-id="60b0c-123">錯誤在 [來源]  資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="60b0c-123">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="60b0c-124">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="60b0c-124">Run the app at a command prompt</span></span>

<span data-ttu-id="60b0c-125">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="60b0c-125">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="60b0c-126">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="60b0c-126">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="60b0c-127">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="60b0c-127">Framework-dependent deployment</span></span>

<span data-ttu-id="60b0c-128">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="60b0c-128">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="60b0c-129">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60b0c-129">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="60b0c-130">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="60b0c-130">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="60b0c-131">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="60b0c-131">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="60b0c-132">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="60b0c-132">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="60b0c-133">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="60b0c-133">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="60b0c-134">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="60b0c-134">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="60b0c-135">自封式部署</span><span class="sxs-lookup"><span data-stu-id="60b0c-135">Self-contained deployment</span></span>

<span data-ttu-id="60b0c-136">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="60b0c-136">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="60b0c-137">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="60b0c-137">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="60b0c-138">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="60b0c-138">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="60b0c-139">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="60b0c-139">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="60b0c-140">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="60b0c-140">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="60b0c-141">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="60b0c-141">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="60b0c-142">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="60b0c-142">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="60b0c-143">ASP.NET Core 模組 stdout 記錄檔</span><span class="sxs-lookup"><span data-stu-id="60b0c-143">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="60b0c-144">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="60b0c-144">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="60b0c-145">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="60b0c-145">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="60b0c-146">如果 [logs]  資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="60b0c-146">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="60b0c-147">如需有關如何讓 MSBuild 在部署中自動建立 [logs]  資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="60b0c-147">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="60b0c-148">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="60b0c-148">Edit the *web.config* file.</span></span> <span data-ttu-id="60b0c-149">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs]  資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="60b0c-149">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="60b0c-150">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="60b0c-150">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="60b0c-151">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="60b0c-151">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="60b0c-152">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="60b0c-152">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="60b0c-153">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="60b0c-153">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="60b0c-154">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="60b0c-154">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="60b0c-155">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="60b0c-155">Make a request to the app.</span></span>
1. <span data-ttu-id="60b0c-156">瀏覽至 [logs]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="60b0c-156">Navigate to the *logs* folder.</span></span> <span data-ttu-id="60b0c-157">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="60b0c-157">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="60b0c-158">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="60b0c-158">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="60b0c-159">完成疑難排解時，請停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="60b0c-159">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="60b0c-160">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="60b0c-160">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="60b0c-161">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="60b0c-161">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="60b0c-162">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="60b0c-162">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="60b0c-163">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="60b0c-163">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="60b0c-164">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="60b0c-164">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="60b0c-165">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="60b0c-165">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="60b0c-166">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="60b0c-166">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="60b0c-167">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="60b0c-167">Enable the Developer Exception Page</span></span>

<span data-ttu-id="60b0c-168">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60b0c-168">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="60b0c-169">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="60b0c-169">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

<span data-ttu-id="60b0c-170">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="60b0c-170">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="60b0c-171">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="60b0c-171">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="60b0c-172">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="60b0c-172">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="60b0c-173">常見的啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="60b0c-173">Common startup errors</span></span>

<span data-ttu-id="60b0c-174">請參閱 <xref:host-and-deploy/azure-iis-errors-reference>。</span><span class="sxs-lookup"><span data-stu-id="60b0c-174">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="60b0c-175">參考主題涵蓋了大多數導致應用程式無法啟動的常見問題。</span><span class="sxs-lookup"><span data-stu-id="60b0c-175">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="60b0c-176">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="60b0c-176">Obtain data from an app</span></span>

<span data-ttu-id="60b0c-177">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="60b0c-177">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="60b0c-178">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="60b0c-178">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="60b0c-179">建立傾印</span><span class="sxs-lookup"><span data-stu-id="60b0c-179">Create a dump</span></span>

<span data-ttu-id="60b0c-180">「傾印」  是系統記憶體的快照集，可協助您判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="60b0c-180">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="60b0c-181">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="60b0c-181">App crashes or encounters an exception</span></span>

<span data-ttu-id="60b0c-182">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="60b0c-182">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="60b0c-183">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="60b0c-183">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="60b0c-184">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="60b0c-184">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="60b0c-185">執行 [EnableDumps PowerShell 指令碼](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="60b0c-185">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="60b0c-186">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="60b0c-186">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="60b0c-187">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="60b0c-187">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="60b0c-188">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="60b0c-188">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="60b0c-189">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="60b0c-189">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="60b0c-190">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="60b0c-190">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="60b0c-191">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="60b0c-191">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="60b0c-192">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="60b0c-192">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="60b0c-193">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="60b0c-193">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="60b0c-194">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="60b0c-194">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="60b0c-195">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="60b0c-195">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="60b0c-196">當應用程式「停止回應」  (停止回應但未損毀)、在啟動期間失敗，或正常執行時，請參閱[使用者模式傾印檔案：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)，選取適當的工具來產生傾印。</span><span class="sxs-lookup"><span data-stu-id="60b0c-196">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="60b0c-197">分析傾印</span><span class="sxs-lookup"><span data-stu-id="60b0c-197">Analyze the dump</span></span>

<span data-ttu-id="60b0c-198">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="60b0c-198">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="60b0c-199">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="60b0c-199">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="60b0c-200">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="60b0c-200">Remote debugging</span></span>

<span data-ttu-id="60b0c-201">請參閱 Visual Studio 文件中的[在 Visual Studio 2017 中針對遠端 IIS 電腦上的 ASP.NET Core 進行遠端偵錯](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) \(機器翻譯\)。</span><span class="sxs-lookup"><span data-stu-id="60b0c-201">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="60b0c-202">Application Insights</span><span class="sxs-lookup"><span data-stu-id="60b0c-202">Application Insights</span></span>

<span data-ttu-id="60b0c-203">[Application Insights](/azure/application-insights/) 提供來自 IIS 所裝載應用程式的遙測資料，包括錯誤記錄和報告功能。</span><span class="sxs-lookup"><span data-stu-id="60b0c-203">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="60b0c-204">Application Insights 只能針對在應用程式啟動之後，應用程式記錄功能變成可用時所發生的錯誤提出報告。</span><span class="sxs-lookup"><span data-stu-id="60b0c-204">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="60b0c-205">如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="60b0c-205">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="60b0c-206">額外建議</span><span class="sxs-lookup"><span data-stu-id="60b0c-206">Additional advice</span></span>

<span data-ttu-id="60b0c-207">有時，在升級開發電腦上的 .NET Core SDK 或應用程式內的套件版本之後，正常運作的應用程式便立即發生失敗。</span><span class="sxs-lookup"><span data-stu-id="60b0c-207">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="60b0c-208">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="60b0c-208">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="60b0c-209">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="60b0c-209">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="60b0c-210">刪除 [bin]  和 [obj]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="60b0c-210">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="60b0c-211">清除位於 *%UserProfile%\\.nuget\\packages* 和 *%LocalAppData%\\Nuget\\v3-cache*的套件快取。</span><span class="sxs-lookup"><span data-stu-id="60b0c-211">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="60b0c-212">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="60b0c-212">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="60b0c-213">先確認已將伺服器上先前的部署完全刪除，再重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="60b0c-213">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="60b0c-214">有一個便利的方式可以清除套件快取，就是從命令提示字元執行 `dotnet nuget locals all --clear`。</span><span class="sxs-lookup"><span data-stu-id="60b0c-214">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="60b0c-215">您也可以使用 [nuget.exe](https://www.nuget.org/downloads) 工具並執行 `nuget locals all -clear` 命令，來清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="60b0c-215">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="60b0c-216">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="60b0c-216">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60b0c-217">其他資源</span><span class="sxs-lookup"><span data-stu-id="60b0c-217">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
