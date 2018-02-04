---
title: "疑難排解 Azure App Service 上的 ASP.NET Core"
author: guardrex
description: "了解如何診斷 ASP.NET Core Azure App Service 部署的問題。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 144af8e93bb935d07fd064d5f45b40faea4a2664
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a><span data-ttu-id="2098d-103">疑難排解 Azure App Service 上的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2098d-103">Troubleshoot ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="2098d-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2098d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2098d-105">本文說明如何診斷 ASP.NET Core 應用程式使用 Azure 應用程式服務的診斷工具的啟動問題。</span><span class="sxs-lookup"><span data-stu-id="2098d-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue using Azure App Service's diagnostic tools.</span></span> <span data-ttu-id="2098d-106">如需其他疑難排解建議，請參閱[Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)和[如何： 監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)Azure 文件中。</span><span class="sxs-lookup"><span data-stu-id="2098d-106">For additional troubleshooting advice, see [Azure App Service diagnostics overview](/azure/app-service/app-service-diagnostics) and [How to: Monitor Apps in Azure App Service](/azure/app-service/web-sites-monitor) in the Azure documentation.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="2098d-107">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="2098d-107">App startup errors</span></span>

<span data-ttu-id="2098d-108">**502.5 處理程序失敗**</span><span class="sxs-lookup"><span data-stu-id="2098d-108">**502.5 Process Failure**</span></span>  
<span data-ttu-id="2098d-109">工作者處理序會失敗。</span><span class="sxs-lookup"><span data-stu-id="2098d-109">The worker process fails.</span></span> <span data-ttu-id="2098d-110">無法啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="2098d-110">The app doesn't start.</span></span>

<span data-ttu-id="2098d-111">[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)嘗試啟動背景工作處理序，但它無法啟動。</span><span class="sxs-lookup"><span data-stu-id="2098d-111">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="2098d-112">經常檢查應用程式事件記錄檔可協助疑難排解這個問題類型。</span><span class="sxs-lookup"><span data-stu-id="2098d-112">Examining the Application Event Log often helps troubleshoot this type of problem.</span></span> <span data-ttu-id="2098d-113">存取記錄檔中會說明[應用程式事件記錄檔](#application-event-log)> 一節。</span><span class="sxs-lookup"><span data-stu-id="2098d-113">Accessing the log is explained in the [Application Event Log](#application-event-log) section.</span></span>

<span data-ttu-id="2098d-114">*502.5 的處理序失敗*設定不正確的應用程式造成工作者處理序失敗時傳回錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="2098d-114">The *502.5 Process Failure* error page is returned when a misconfigured app causes the worker process to fail:</span></span>

![瀏覽器視窗中顯示 502.5 處理序失敗的頁面](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="2098d-116">**500 內部伺服器錯誤**</span><span class="sxs-lookup"><span data-stu-id="2098d-116">**500 Internal Server Error**</span></span>  
<span data-ttu-id="2098d-117">應用程式啟動，但因為錯誤而造成伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="2098d-117">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="2098d-118">在啟動期間，或建立回應時，應用程式的程式碼中會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="2098d-118">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="2098d-119">回應可能會包含任何內容，或回應可能會顯示為*500 內部伺服器錯誤*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="2098d-119">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="2098d-120">應用程式事件記錄檔通常會指出應用程式正常啟動。</span><span class="sxs-lookup"><span data-stu-id="2098d-120">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="2098d-121">從伺服器的觀點來看，這是正確。</span><span class="sxs-lookup"><span data-stu-id="2098d-121">From the server's perspective, that's correct.</span></span> <span data-ttu-id="2098d-122">應用程式沒有開始，但它無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="2098d-122">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="2098d-123">[Kudu 主控台中執行應用程式](#run-the-app-in-the-kudu-console)或[啟用 ASP.NET 核心模組 stdout 記錄](#aspnet-core-module-stdout-log)來疑難排解問題。</span><span class="sxs-lookup"><span data-stu-id="2098d-123">[Run the app in the Kudu console](#run-the-app-in-the-kudu-console) or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="2098d-124">疑難排解應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="2098d-124">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="2098d-125">應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="2098d-125">Application Event Log</span></span>

<span data-ttu-id="2098d-126">若要存取應用程式事件記錄檔，請使用**診斷並解決問題**刀鋒視窗，在 Azure 入口網站：</span><span class="sxs-lookup"><span data-stu-id="2098d-126">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal :</span></span>

1. <span data-ttu-id="2098d-127">在 Azure 入口網站中，開啟 [應用程式] 刀鋒視窗中的**應用程式服務**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="2098d-127">In the Azure portal, open the app's blade in the **App Services** blade.</span></span>
1. <span data-ttu-id="2098d-128">選取**診斷並解決問題**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="2098d-128">Select the **Diagnose and solve problems** blade.</span></span>
1. <span data-ttu-id="2098d-129">在下**選取問題類別**，選取**Web 應用程式向** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2098d-129">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="2098d-130">在下**建議的解決方案**，開啟的窗格**開啟應用程式事件記錄檔**。</span><span class="sxs-lookup"><span data-stu-id="2098d-130">Under **Suggested Solutions**, open the pane for **Open Application Event Logs**.</span></span> <span data-ttu-id="2098d-131">選取**開啟應用程式事件記錄檔** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2098d-131">Select the **Open Application Event Logs** button.</span></span>
1. <span data-ttu-id="2098d-132">檢查所提供的最新錯誤*IIS AspNetCoreModule*中**來源**資料行。</span><span class="sxs-lookup"><span data-stu-id="2098d-132">Examine the latest error provided by the *IIS AspNetCoreModule* in the **Source** column.</span></span>

<span data-ttu-id="2098d-133">除了使用**診斷並解決問題**刀鋒視窗會檢查直接使用的應用程式事件記錄檔[Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="2098d-133">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="2098d-134">選取**進階工具**刀鋒視窗中的**開發工具**區域。</span><span class="sxs-lookup"><span data-stu-id="2098d-134">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="2098d-135">選取**移&rarr;**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="2098d-135">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="2098d-136">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="2098d-136">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="2098d-137">使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。</span><span class="sxs-lookup"><span data-stu-id="2098d-137">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="2098d-138">開啟**LogFiles**資料夾。</span><span class="sxs-lookup"><span data-stu-id="2098d-138">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="2098d-139">選取鉛筆圖示旁*eventlog.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="2098d-139">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="2098d-140">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="2098d-140">Examine the log.</span></span> <span data-ttu-id="2098d-141">捲動至底部，以查看最新的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="2098d-141">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="2098d-142">Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="2098d-142">Run the app in the Kudu console</span></span>

<span data-ttu-id="2098d-143">許多的啟動錯誤不會產生應用程式事件日誌中的有用資訊。</span><span class="sxs-lookup"><span data-stu-id="2098d-143">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="2098d-144">您可以在執行應用程式[Kudu](https://github.com/projectkudu/kudu/wiki)發現錯誤的遠端執行主控台：</span><span class="sxs-lookup"><span data-stu-id="2098d-144">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="2098d-145">選取**進階工具**刀鋒視窗中的**開發工具**區域。</span><span class="sxs-lookup"><span data-stu-id="2098d-145">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="2098d-146">選取**移&rarr;**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="2098d-146">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="2098d-147">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="2098d-147">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="2098d-148">使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。</span><span class="sxs-lookup"><span data-stu-id="2098d-148">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="2098d-149">開啟資料夾的路徑**網站** > **wwwroot**。</span><span class="sxs-lookup"><span data-stu-id="2098d-149">Open the folders to the path **site** > **wwwroot**.</span></span>
1. <span data-ttu-id="2098d-150">在主控台中，執行應用程式藉由執行應用程式的組件*dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="2098d-150">In the console, run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="2098d-151">下列命令，以取代應用程式的組件名稱`<assembly_name>`:</span><span class="sxs-lookup"><span data-stu-id="2098d-151">In the following command, substitute the name of the app's assembly for `<assembly_name>`:</span></span>
   ```console
   dotnet .\<assembly_name>.dll
   ```
1. <span data-ttu-id="2098d-152">主控台應用程式，並顯示任何錯誤，從輸出送到 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="2098d-152">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="2098d-153">ASP.NET 核心模組 stdout 記錄檔</span><span class="sxs-lookup"><span data-stu-id="2098d-153">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="2098d-154">ASP.NET 核心模組 stdout 記錄通常會記錄應用程式事件日誌中找不到有用的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="2098d-154">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="2098d-155">若要啟用，並檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="2098d-155">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="2098d-156">瀏覽至**診斷並解決問題**在 Azure 入口網站的刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="2098d-156">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="2098d-157">在下**選取問題類別**，選取**Web 應用程式向** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2098d-157">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="2098d-158">在下**建議的解決方案** > **啟用 Stdout 記錄檔重新導向**，選取按鈕以**開啟 Kudu 主控台編輯 Web.Config**。</span><span class="sxs-lookup"><span data-stu-id="2098d-158">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="2098d-159">在 Kudu**診斷主控台**，開啟的資料夾路徑**網站** > **wwwroot**。</span><span class="sxs-lookup"><span data-stu-id="2098d-159">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="2098d-160">向下捲動至顯示*web.config*檔案清單的底部。</span><span class="sxs-lookup"><span data-stu-id="2098d-160">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="2098d-161">按一下鉛筆圖示旁*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="2098d-161">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="2098d-162">設定**stdoutLogEnabled**至`true`並變更**stdoutLogFile**路徑： `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="2098d-162">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="2098d-163">選取**儲存**來儲存已更新*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="2098d-163">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="2098d-164">應用程式提出要求。</span><span class="sxs-lookup"><span data-stu-id="2098d-164">Make a request to the app.</span></span>
1. <span data-ttu-id="2098d-165">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="2098d-165">Return to the Azure portal.</span></span> <span data-ttu-id="2098d-166">選取**進階工具**刀鋒視窗中的**開發工具**區域。</span><span class="sxs-lookup"><span data-stu-id="2098d-166">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="2098d-167">選取**移&rarr;**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="2098d-167">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="2098d-168">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="2098d-168">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="2098d-169">使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。</span><span class="sxs-lookup"><span data-stu-id="2098d-169">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="2098d-170">選取**LogFiles**資料夾。</span><span class="sxs-lookup"><span data-stu-id="2098d-170">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="2098d-171">檢查**Modified**資料行，然後選取鉛筆圖示，以編輯 stdout 記錄的最新的修改日期。</span><span class="sxs-lookup"><span data-stu-id="2098d-171">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="2098d-172">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="2098d-172">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="2098d-173">**重要 ！**</span><span class="sxs-lookup"><span data-stu-id="2098d-173">**Important!**</span></span> <span data-ttu-id="2098d-174">停用 stdout 記錄完成疑難排解時。</span><span class="sxs-lookup"><span data-stu-id="2098d-174">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="2098d-175">在 Kudu**診斷主控台**，返回路徑**網站** > **wwwroot**以顯示*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="2098d-175">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="2098d-176">開啟**web.config**檔案一次選取鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="2098d-176">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="2098d-177">設定**stdoutLogEnabled**至`false`。</span><span class="sxs-lookup"><span data-stu-id="2098d-177">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="2098d-178">選取**儲存**儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="2098d-178">Select **Save** to save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="2098d-179">若要停用 stdout 記錄的失敗可能會導致應用程式或伺服器失敗。</span><span class="sxs-lookup"><span data-stu-id="2098d-179">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="2098d-180">沒有記錄檔的大小沒有限制或建立的記錄檔的數目。</span><span class="sxs-lookup"><span data-stu-id="2098d-180">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="2098d-181">例行記錄中的 ASP.NET Core 應用程式、 使用限制記錄檔大小，並且會旋轉記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="2098d-181">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="2098d-182">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="2098d-182">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="2098d-183">常見的啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="2098d-183">Common startup errors</span></span> 

<span data-ttu-id="2098d-184">請參閱[ASP.NET Core 常見錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)。</span><span class="sxs-lookup"><span data-stu-id="2098d-184">See the [ASP.NET Core common errors reference](xref:host-and-deploy/azure-iis-errors-reference).</span></span> <span data-ttu-id="2098d-185">參考主題會說明最常見的問題阻礙應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="2098d-185">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="process-dump-for-a-slow-or-hanging-app"></a><span data-ttu-id="2098d-186">較慢或無回應的應用程式的處理序傾印</span><span class="sxs-lookup"><span data-stu-id="2098d-186">Process dump for a slow or hanging app</span></span>

<span data-ttu-id="2098d-187">當應用程式回應變慢或停止回應的要求時，請參閱[疑難排解緩慢的 web 應用程式在 Azure App Service 中的效能問題](/azure/app-service/app-service-web-troubleshoot-performance-degradation)偵錯指引。</span><span class="sxs-lookup"><span data-stu-id="2098d-187">When an app responds slowly or hangs on a request, see [Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) for debugging guidance.</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="2098d-188">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="2098d-188">Remote debugging</span></span>

<span data-ttu-id="2098d-189">請參閱[遠端偵錯 web 應用程式的疑難排解中使用 Visual Studio 的 Azure App Service web 應用程式 區段](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)Azure 文件中。</span><span class="sxs-lookup"><span data-stu-id="2098d-189">See [Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) in the Azure documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="2098d-190">Application Insights</span><span class="sxs-lookup"><span data-stu-id="2098d-190">Application Insights</span></span>

<span data-ttu-id="2098d-191">[Application Insights](https://azure.microsoft.com/services/application-insights/)提供裝載於 Azure 應用程式服務，包括錯誤記錄和報告功能的應用程式的遙測。</span><span class="sxs-lookup"><span data-stu-id="2098d-191">[Application Insights](https://azure.microsoft.com/services/application-insights/) provides telemetry from apps hosted in the Azure App Service, including error logging and reporting features.</span></span> <span data-ttu-id="2098d-192">Application Insights 只可以在應用程式啟動時的應用程式記錄功能會變成可用之後，會發生的錯誤報告。</span><span class="sxs-lookup"><span data-stu-id="2098d-192">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="2098d-193">如需詳細資訊，請參閱[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="2098d-193">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="monitoring-blades"></a><span data-ttu-id="2098d-194">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="2098d-194">Monitoring blades</span></span>

<span data-ttu-id="2098d-195">監視刀鋒提供疑難排解體驗，本主題稍早所述之方法的替代方案。</span><span class="sxs-lookup"><span data-stu-id="2098d-195">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="2098d-196">可用來診斷 500 系列錯誤設定了這些刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="2098d-196">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="2098d-197">確認已安裝 ASP.NET Core 擴充功能。</span><span class="sxs-lookup"><span data-stu-id="2098d-197">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="2098d-198">如果尚未安裝擴充功能，請手動進行安裝：</span><span class="sxs-lookup"><span data-stu-id="2098d-198">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="2098d-199">在**開發工具**刀鋒視窗區段中，選取**延伸**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="2098d-199">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="2098d-200">**ASP.NET Core Extensions**應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="2098d-200">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="2098d-201">如果尚未安裝擴充功能，請選取**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2098d-201">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="2098d-202">選擇**ASP.NET Core Extensions**從清單中。</span><span class="sxs-lookup"><span data-stu-id="2098d-202">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="2098d-203">選取**確定**接受法律合約。</span><span class="sxs-lookup"><span data-stu-id="2098d-203">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="2098d-204">選取**確定**上**將延伸加入**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="2098d-204">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="2098d-205">資訊的快顯訊息表示已成功安裝擴充功能時。</span><span class="sxs-lookup"><span data-stu-id="2098d-205">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="2098d-206">如果未啟用 stdout 記錄，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="2098d-206">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="2098d-207">在 Azure 入口網站中，選取**進階工具**刀鋒視窗中的**開發工具**區域。</span><span class="sxs-lookup"><span data-stu-id="2098d-207">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="2098d-208">選取**移&rarr;**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="2098d-208">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="2098d-209">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="2098d-209">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="2098d-210">使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。</span><span class="sxs-lookup"><span data-stu-id="2098d-210">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="2098d-211">開啟資料夾的路徑**網站** > **wwwroot**和向下捲動至顯示*web.config*檔案清單的底部。</span><span class="sxs-lookup"><span data-stu-id="2098d-211">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="2098d-212">按一下鉛筆圖示旁*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="2098d-212">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="2098d-213">設定**stdoutLogEnabled**至`true`並變更**stdoutLogFile**路徑： `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="2098d-213">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="2098d-214">選取**儲存**來儲存已更新*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="2098d-214">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="2098d-215">若要啟用診斷記錄，繼續進行：</span><span class="sxs-lookup"><span data-stu-id="2098d-215">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="2098d-216">在 Azure 入口網站中，選取**診斷記錄檔**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="2098d-216">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="2098d-217">選取**上**切換**的應用程式記錄 （檔案系統）**和**詳細錯誤訊息**。</span><span class="sxs-lookup"><span data-stu-id="2098d-217">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="2098d-218">選取**儲存**刀鋒視窗頂端的按鈕。</span><span class="sxs-lookup"><span data-stu-id="2098d-218">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="2098d-219">若要包含失敗的要求追蹤，也就是失敗的要求事件緩衝處理 (FREB) 記錄中，選取**上**切換**追蹤失敗的要求**。</span><span class="sxs-lookup"><span data-stu-id="2098d-219">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span> 
1. <span data-ttu-id="2098d-220">選取**記錄檔資料流**刀鋒視窗，立即會列在底下**診斷記錄檔**在入口網站的刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="2098d-220">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="2098d-221">應用程式提出要求。</span><span class="sxs-lookup"><span data-stu-id="2098d-221">Make a request to the app.</span></span>
1. <span data-ttu-id="2098d-222">在記錄檔之資料流資料，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="2098d-222">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="2098d-223">**重要 ！**</span><span class="sxs-lookup"><span data-stu-id="2098d-223">**Important!**</span></span> <span data-ttu-id="2098d-224">請務必停用 stdout 記錄完成疑難排解時。</span><span class="sxs-lookup"><span data-stu-id="2098d-224">Be sure to disable stdout logging when troubleshooting is complete.</span></span> <span data-ttu-id="2098d-225">中的指示，請參閱 < [ASP.NET 核心模組 stdout 記錄](#aspnet-core-module-stdout-log)> 一節。</span><span class="sxs-lookup"><span data-stu-id="2098d-225">See the instructions in the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) section.</span></span>

<span data-ttu-id="2098d-226">若要檢視失敗的要求追蹤記錄檔 （FREB 記錄檔）：</span><span class="sxs-lookup"><span data-stu-id="2098d-226">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="2098d-227">瀏覽至**診斷並解決問題**在 Azure 入口網站的刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="2098d-227">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="2098d-228">選取**失敗的要求追蹤記錄**從**支援工具**區域的 [資訊看板]。</span><span class="sxs-lookup"><span data-stu-id="2098d-228">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="2098d-229">請參閱[失敗要求追蹤的 Azure App Service 主題中的 web 應用程式啟用診斷記錄 區段](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[在 Azure 中的 Web 應用程式的應用程式效能常見問題集： 如何開啟追蹤失敗的要求？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="2098d-229">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="2098d-230">如需詳細資訊，請參閱[啟用 Azure App Service 中的 web 應用程式的診斷記錄](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="2098d-230">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="2098d-231">若要停用 stdout 記錄的失敗可能會導致應用程式或伺服器失敗。</span><span class="sxs-lookup"><span data-stu-id="2098d-231">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="2098d-232">沒有記錄檔的大小沒有限制或建立的記錄檔的數目。</span><span class="sxs-lookup"><span data-stu-id="2098d-232">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="2098d-233">例行記錄中的 ASP.NET Core 應用程式、 使用限制記錄檔大小，並且會旋轉記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="2098d-233">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="2098d-234">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="2098d-234">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2098d-235">其他資源</span><span class="sxs-lookup"><span data-stu-id="2098d-235">Additional resources</span></span>

* [<span data-ttu-id="2098d-236">Introduction to ASP.NET Core 中的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="2098d-236">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)
* [<span data-ttu-id="2098d-237">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="2098d-237">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="2098d-238">疑難排解 Azure App Service 使用 Visual Studio 中的 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="2098d-238">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="2098d-239">疑難排解 「 502 不正確的閘道 」 和 「 503 服務無法使用 「 Azure web 應用程式中的 HTTP 錯誤</span><span class="sxs-lookup"><span data-stu-id="2098d-239">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="2098d-240">Azure App Service 中的速度慢的 web 應用程式效能問題的疑難排解</span><span class="sxs-lookup"><span data-stu-id="2098d-240">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="2098d-241">在 Azure 中的 Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="2098d-241">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="2098d-242">Azure 星期五： Azure 的應用程式服務診斷和疑難排解體驗 （12 分鐘的影片）</span><span class="sxs-lookup"><span data-stu-id="2098d-242">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
