---
title: 針對 Azure App Service 上的 ASP.NET Core 進行疑難排解
author: guardrex
description: 了解如何診斷 ASP.NET Core Azure App Service 部署的問題。
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: d78499c1a82a011239f6b62b546f304a5d5017e2
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313751"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a><span data-ttu-id="b88ad-103">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b88ad-103">Troubleshoot ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="b88ad-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b88ad-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="b88ad-105">本文說明如何使用 Azure App Service 診斷工具來診斷 ASP.NET Core 應用程式啟動問題。</span><span class="sxs-lookup"><span data-stu-id="b88ad-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue using Azure App Service's diagnostic tools.</span></span> <span data-ttu-id="b88ad-106">如需其他疑難排解建議，請參閱 Azure 文件中的 [Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)和[如何：監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)。</span><span class="sxs-lookup"><span data-stu-id="b88ad-106">For additional troubleshooting advice, see [Azure App Service diagnostics overview](/azure/app-service/app-service-diagnostics) and [How to: Monitor Apps in Azure App Service](/azure/app-service/web-sites-monitor) in the Azure documentation.</span></span>

<span data-ttu-id="b88ad-107">其他疑難排解主題：</span><span class="sxs-lookup"><span data-stu-id="b88ad-107">Additional troubleshooting topics:</span></span>

* <span data-ttu-id="b88ad-108">IIS 也會使用 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)來裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="b88ad-108">IIS also uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host apps.</span></span> <span data-ttu-id="b88ad-109">如需有關 IIS 的疑難排解資訊，請參閱 <xref:host-and-deploy/iis/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="b88ad-109">For troubleshooting advice that pertains specifically to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>
* <span data-ttu-id="b88ad-110"><xref:fundamentals/error-handling> 涵蓋在本機系統上進行開發時，如何處理 ASP.NET Core 應用程式中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b88ad-110"><xref:fundamentals/error-handling> covers how to handle errors in ASP.NET Core apps during development on a local system.</span></span>
* <span data-ttu-id="b88ad-111">[了解如何使用 Visual Studio 進行偵錯](/visualstudio/debugger/getting-started-with-the-debugger)介紹 Visual Studio 偵錯工具的功能。</span><span class="sxs-lookup"><span data-stu-id="b88ad-111">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) introduces the features of the Visual Studio debugger.</span></span>
* <span data-ttu-id="b88ad-112">[使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\) 說明 Visual Studio Code 中內建的偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="b88ad-112">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) describes the debugging support built into Visual Studio Code.</span></span>

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="b88ad-113">針對應用程式啟動錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b88ad-113">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="b88ad-114">應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="b88ad-114">Application Event Log</span></span>

<span data-ttu-id="b88ad-115">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題]  刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="b88ad-115">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="b88ad-116">在 Azure 入口網站的 [應用程式服務]  中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="b88ad-116">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="b88ad-117">選取 [診斷並解決問題]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-117">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="b88ad-118">選取 [診斷工具]  標題。</span><span class="sxs-lookup"><span data-stu-id="b88ad-118">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="b88ad-119">在 [支援工具]  下，選取 [應用程式事件]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-119">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="b88ad-120">檢查 [來源]  資料行中 *IIS AspNetCoreModule* 或 *IIS AspNetCoreModule V2* 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="b88ad-120">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="b88ad-121">除了使用 [診斷並解決問題]  刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="b88ad-121">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="b88ad-122">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-122">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b88ad-123">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-123">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b88ad-124">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b88ad-124">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b88ad-125">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-125">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b88ad-126">開啟 [LogFiles]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="b88ad-126">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="b88ad-127">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b88ad-127">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="b88ad-128">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b88ad-128">Examine the log.</span></span> <span data-ttu-id="b88ad-129">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="b88ad-129">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="b88ad-130">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b88ad-130">Run the app in the Kudu console</span></span>

<span data-ttu-id="b88ad-131">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="b88ad-131">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="b88ad-132">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="b88ad-132">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="b88ad-133">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-133">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b88ad-134">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-134">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b88ad-135">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b88ad-135">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b88ad-136">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-136">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="b88ad-137">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="b88ad-137">Test a 32-bit (x86) app</span></span>

##### <a name="current-release"></a><span data-ttu-id="b88ad-138">目前的版本</span><span class="sxs-lookup"><span data-stu-id="b88ad-138">Current release</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="b88ad-139">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="b88ad-139">Run the app:</span></span>
   * <span data-ttu-id="b88ad-140">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b88ad-140">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="b88ad-141">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b88ad-141">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="b88ad-142">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b88ad-142">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a><span data-ttu-id="b88ad-143">在預覽版上執行的架構相依部署</span><span class="sxs-lookup"><span data-stu-id="b88ad-143">Framework-dependent deployment running on a preview release</span></span>

<span data-ttu-id="b88ad-144">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="b88ad-144">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="b88ad-145">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="b88ad-145">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="b88ad-146">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b88ad-146">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="b88ad-147">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b88ad-147">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="b88ad-148">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="b88ad-148">Test a 64-bit (x64) app</span></span>

##### <a name="current-release"></a><span data-ttu-id="b88ad-149">目前的版本</span><span class="sxs-lookup"><span data-stu-id="b88ad-149">Current release</span></span>

* <span data-ttu-id="b88ad-150">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b88ad-150">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="b88ad-151">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b88ad-151">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="b88ad-152">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b88ad-152">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="b88ad-153">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="b88ad-153">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="b88ad-154">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b88ad-154">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a><span data-ttu-id="b88ad-155">在預覽版上執行的架構相依部署</span><span class="sxs-lookup"><span data-stu-id="b88ad-155">Framework-dependent deployment running on a preview release</span></span>

<span data-ttu-id="b88ad-156">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="b88ad-156">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="b88ad-157">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="b88ad-157">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="b88ad-158">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b88ad-158">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="b88ad-159">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b88ad-159">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="b88ad-160">ASP.NET Core 模組 stdout 記錄檔</span><span class="sxs-lookup"><span data-stu-id="b88ad-160">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="b88ad-161">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="b88ad-161">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="b88ad-162">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b88ad-162">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b88ad-163">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b88ad-163">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="b88ad-164">在 [選取問題類別]  底下，選取 [Web 應用程式停止運作]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-164">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="b88ad-165">在 [建議的解決方案]   > [啟用 Stdout 記錄檔重新導向]  底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-165">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="b88ad-166">在 Kudu [診斷主控台]  中，將資料夾開啟至路徑 [site]   > [wwwroot]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-166">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b88ad-167">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b88ad-167">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="b88ad-168">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b88ad-168">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="b88ad-169">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="b88ad-169">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="b88ad-170">選取 [儲存]  以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b88ad-170">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="b88ad-171">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b88ad-171">Make a request to the app.</span></span>
1. <span data-ttu-id="b88ad-172">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="b88ad-172">Return to the Azure portal.</span></span> <span data-ttu-id="b88ad-173">選取 [開發工具]  區域中的 [進階工具]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b88ad-173">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="b88ad-174">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-174">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b88ad-175">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b88ad-175">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b88ad-176">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-176">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b88ad-177">選取 [LogFiles]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="b88ad-177">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="b88ad-178">檢查 [修改時間]  資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b88ad-178">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="b88ad-179">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="b88ad-179">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="b88ad-180">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="b88ad-180">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="b88ad-181">在 Kudu [診斷主控台]  中，返回路徑 [site]   > [wwwroot]  以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b88ad-181">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="b88ad-182">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="b88ad-182">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="b88ad-183">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="b88ad-183">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="b88ad-184">選取 [儲存]  以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="b88ad-184">Select **Save** to save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="b88ad-185">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b88ad-185">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b88ad-186">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b88ad-186">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="b88ad-187">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="b88ad-187">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="b88ad-188">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b88ad-188">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b88ad-189">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b88ad-189">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log"></a><span data-ttu-id="b88ad-190">ASP.NET Core 模組偵錯記錄</span><span class="sxs-lookup"><span data-stu-id="b88ad-190">ASP.NET Core Module debug log</span></span>

<span data-ttu-id="b88ad-191">ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。</span><span class="sxs-lookup"><span data-stu-id="b88ad-191">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="b88ad-192">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b88ad-192">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b88ad-193">若要啟用增強型診斷記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="b88ad-193">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="b88ad-194">遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="b88ad-194">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="b88ad-195">重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="b88ad-195">Redeploy the app.</span></span>
   * <span data-ttu-id="b88ad-196">使用 Kudu 主控台，將[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所顯示的 `<handlerSettings>` 新增至即時應用程式的 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="b88ad-196">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="b88ad-197">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-197">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b88ad-198">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-198">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b88ad-199">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b88ad-199">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="b88ad-200">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-200">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="b88ad-201">將資料夾開啟至路徑 [site]   > [wwwroot]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-201">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b88ad-202">選取鉛筆圖示來編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b88ad-202">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="b88ad-203">新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。</span><span class="sxs-lookup"><span data-stu-id="b88ad-203">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="b88ad-204">選取 [儲存]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-204">Select the **Save** button.</span></span>
1. <span data-ttu-id="b88ad-205">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-205">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b88ad-206">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-206">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b88ad-207">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b88ad-207">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b88ad-208">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-208">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b88ad-209">將資料夾開啟至路徑 [site]   > [wwwroot]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-209">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b88ad-210">如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。</span><span class="sxs-lookup"><span data-stu-id="b88ad-210">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="b88ad-211">如果已提供路徑，請巡覽至記錄檔的位置。</span><span class="sxs-lookup"><span data-stu-id="b88ad-211">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="b88ad-212">使用檔案名稱旁的鉛筆圖示來開啟記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b88ad-212">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="b88ad-213">完成疑難排解時，請停用偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="b88ad-213">Disable debug logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="b88ad-214">若要停用增強型偵錯記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="b88ad-214">To disable the enhanced debug log, perform either of the following:</span></span>
   * <span data-ttu-id="b88ad-215">從 *web.config* 檔案本機移除 `<handlerSettings>` 並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="b88ad-215">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
   * <span data-ttu-id="b88ad-216">使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。</span><span class="sxs-lookup"><span data-stu-id="b88ad-216">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="b88ad-217">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="b88ad-217">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="b88ad-218">無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b88ad-218">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="b88ad-219">記錄檔大小沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="b88ad-219">There's no limit on log file size.</span></span> <span data-ttu-id="b88ad-220">請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。</span><span class="sxs-lookup"><span data-stu-id="b88ad-220">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="b88ad-221">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b88ad-221">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b88ad-222">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b88ad-222">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

## <a name="common-startup-errors"></a><span data-ttu-id="b88ad-223">常見的啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="b88ad-223">Common startup errors</span></span>

<span data-ttu-id="b88ad-224">請參閱 <xref:host-and-deploy/azure-iis-errors-reference>。</span><span class="sxs-lookup"><span data-stu-id="b88ad-224">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="b88ad-225">參考主題涵蓋了大多數導致應用程式無法啟動的常見問題。</span><span class="sxs-lookup"><span data-stu-id="b88ad-225">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="b88ad-226">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="b88ad-226">Slow or hanging app</span></span>

<span data-ttu-id="b88ad-227">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="b88ad-227">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="b88ad-228">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b88ad-228">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="b88ad-229">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b88ad-229">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="b88ad-230">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="b88ad-230">Remote debugging</span></span>

<span data-ttu-id="b88ad-231">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="b88ad-231">See the following topics:</span></span>

* <span data-ttu-id="b88ad-232">[＜使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式＞的＜遠端偵錯 Web 應用程式＞一節](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure 文件)</span><span class="sxs-lookup"><span data-stu-id="b88ad-232">[Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure documentation)</span></span>
* <span data-ttu-id="b88ad-233">[在 Visual Studio 2017 中針對 Azure 中 IIS 上的 ASP.NET Core 進行遠端偵錯](/visualstudio/debugger/remote-debugging-azure) \(機器翻譯\) (Visual Studio 文件)</span><span class="sxs-lookup"><span data-stu-id="b88ad-233">[Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (Visual Studio documentation)</span></span>

## <a name="application-insights"></a><span data-ttu-id="b88ad-234">Application Insights</span><span class="sxs-lookup"><span data-stu-id="b88ad-234">Application Insights</span></span>

<span data-ttu-id="b88ad-235">[Application Insights](https://azure.microsoft.com/services/application-insights/) 提供來自 Azure App Service 中所裝載應用程式的遙測資料，包括錯誤記錄和報告功能。</span><span class="sxs-lookup"><span data-stu-id="b88ad-235">[Application Insights](https://azure.microsoft.com/services/application-insights/) provides telemetry from apps hosted in the Azure App Service, including error logging and reporting features.</span></span> <span data-ttu-id="b88ad-236">Application Insights 只能針對在應用程式啟動之後，應用程式記錄功能變成可用時所發生的錯誤提出報告。</span><span class="sxs-lookup"><span data-stu-id="b88ad-236">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="b88ad-237">如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="b88ad-237">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="monitoring-blades"></a><span data-ttu-id="b88ad-238">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="b88ad-238">Monitoring blades</span></span>

<span data-ttu-id="b88ad-239">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="b88ad-239">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="b88ad-240">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b88ad-240">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="b88ad-241">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="b88ad-241">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="b88ad-242">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="b88ad-242">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="b88ad-243">在 [開發工具]  刀鋒視窗區段中，選取 [延伸模組]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b88ad-243">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="b88ad-244">[ASP.NET Core 延伸模組]  應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="b88ad-244">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="b88ad-245">如果未安裝這些延伸模組，請選取 [新增]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-245">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="b88ad-246">從清單中選擇 [ASP.NET Core 延伸模組]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-246">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="b88ad-247">選取 [確定]  以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="b88ad-247">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="b88ad-248">在 [新增延伸模組]  刀鋒視窗上，選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-248">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="b88ad-249">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="b88ad-249">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="b88ad-250">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="b88ad-250">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="b88ad-251">在 Azure 入口網站中，選取 [開發工具]  區域中的 [進階工具]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b88ad-251">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="b88ad-252">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-252">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b88ad-253">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b88ad-253">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b88ad-254">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-254">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b88ad-255">將資料夾開啟至路徑 [site]   > [wwwroot]  ，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b88ad-255">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="b88ad-256">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b88ad-256">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="b88ad-257">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="b88ad-257">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="b88ad-258">選取 [儲存]  以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b88ad-258">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="b88ad-259">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="b88ad-259">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="b88ad-260">在 Azure 入口網站中，選取 [診斷記錄檔]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b88ad-260">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="b88ad-261">選取 [應用程式記錄 (檔案系統)]  和 [詳細錯誤訊息]  .的 [開啟]  開關。</span><span class="sxs-lookup"><span data-stu-id="b88ad-261">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="b88ad-262">選取刀鋒視窗頂端的 [儲存]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="b88ad-262">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="b88ad-263">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤]  的 [開啟]  開關。</span><span class="sxs-lookup"><span data-stu-id="b88ad-263">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="b88ad-264">選取 [記錄資料流]  刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔]  刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="b88ad-264">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="b88ad-265">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b88ad-265">Make a request to the app.</span></span>
1. <span data-ttu-id="b88ad-266">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="b88ad-266">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="b88ad-267">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="b88ad-267">Be sure to disable stdout logging when troubleshooting is complete.</span></span> <span data-ttu-id="b88ad-268">請參閱 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)一節中的指示。</span><span class="sxs-lookup"><span data-stu-id="b88ad-268">See the instructions in the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) section.</span></span>

<span data-ttu-id="b88ad-269">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="b88ad-269">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="b88ad-270">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b88ad-270">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="b88ad-271">從資訊看板的 [支援工具]  區域中，選取 [失敗要求追蹤記錄檔]  。</span><span class="sxs-lookup"><span data-stu-id="b88ad-271">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="b88ad-272">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和 [Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="b88ad-272">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="b88ad-273">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="b88ad-273">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="b88ad-274">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b88ad-274">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b88ad-275">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b88ad-275">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="b88ad-276">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b88ad-276">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b88ad-277">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b88ad-277">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b88ad-278">其他資源</span><span class="sxs-lookup"><span data-stu-id="b88ad-278">Additional resources</span></span>

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="b88ad-279">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b88ad-279">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="b88ad-280">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b88ad-280">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="b88ad-281">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b88ad-281">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="b88ad-282">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="b88ad-282">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="b88ad-283">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b88ad-283">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="b88ad-284">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="b88ad-284">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
