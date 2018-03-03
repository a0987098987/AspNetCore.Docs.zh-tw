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
ms.openlocfilehash: 27a46446e9bf63e96eecc392e6d6863e27b34730
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a><span data-ttu-id="af1cd-103">疑難排解 Azure App Service 上的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af1cd-103">Troubleshoot ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="af1cd-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="af1cd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="af1cd-105">本文說明如何診斷 ASP.NET Core 應用程式使用 Azure 應用程式服務的診斷工具的啟動問題。</span><span class="sxs-lookup"><span data-stu-id="af1cd-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue using Azure App Service's diagnostic tools.</span></span> <span data-ttu-id="af1cd-106">如需其他疑難排解建議，請參閱[Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)和[如何： 監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)Azure 文件中。</span><span class="sxs-lookup"><span data-stu-id="af1cd-106">For additional troubleshooting advice, see [Azure App Service diagnostics overview](/azure/app-service/app-service-diagnostics) and [How to: Monitor Apps in Azure App Service](/azure/app-service/web-sites-monitor) in the Azure documentation.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="af1cd-107">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="af1cd-107">App startup errors</span></span>

<span data-ttu-id="af1cd-108">**502.5 處理程序失敗**</span><span class="sxs-lookup"><span data-stu-id="af1cd-108">**502.5 Process Failure**</span></span>  
<span data-ttu-id="af1cd-109">工作者處理序會失敗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-109">The worker process fails.</span></span> <span data-ttu-id="af1cd-110">無法啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="af1cd-110">The app doesn't start.</span></span>

<span data-ttu-id="af1cd-111">[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)嘗試啟動背景工作處理序，但它無法啟動。</span><span class="sxs-lookup"><span data-stu-id="af1cd-111">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="af1cd-112">經常檢查應用程式事件記錄檔可協助疑難排解這個問題類型。</span><span class="sxs-lookup"><span data-stu-id="af1cd-112">Examining the Application Event Log often helps troubleshoot this type of problem.</span></span> <span data-ttu-id="af1cd-113">存取記錄檔中會說明[應用程式事件記錄檔](#application-event-log)> 一節。</span><span class="sxs-lookup"><span data-stu-id="af1cd-113">Accessing the log is explained in the [Application Event Log](#application-event-log) section.</span></span>

<span data-ttu-id="af1cd-114">*502.5 的處理序失敗*設定不正確的應用程式造成工作者處理序失敗時傳回錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="af1cd-114">The *502.5 Process Failure* error page is returned when a misconfigured app causes the worker process to fail:</span></span>

![瀏覽器視窗中顯示 502.5 處理序失敗的頁面](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="af1cd-116">**500 內部伺服器錯誤**</span><span class="sxs-lookup"><span data-stu-id="af1cd-116">**500 Internal Server Error**</span></span>  
<span data-ttu-id="af1cd-117">應用程式啟動，但因為錯誤而造成伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="af1cd-117">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="af1cd-118">在啟動期間，或建立回應時，應用程式的程式碼中會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="af1cd-118">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="af1cd-119">回應可能會包含任何內容，或回應可能會顯示為*500 內部伺服器錯誤*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="af1cd-119">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="af1cd-120">應用程式事件記錄檔通常會指出應用程式正常啟動。</span><span class="sxs-lookup"><span data-stu-id="af1cd-120">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="af1cd-121">從伺服器的觀點來看，這是正確。</span><span class="sxs-lookup"><span data-stu-id="af1cd-121">From the server's perspective, that's correct.</span></span> <span data-ttu-id="af1cd-122">應用程式沒有開始，但它無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="af1cd-122">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="af1cd-123">[Kudu 主控台中執行應用程式](#run-the-app-in-the-kudu-console)或[啟用 ASP.NET 核心模組 stdout 記錄](#aspnet-core-module-stdout-log)來疑難排解問題。</span><span class="sxs-lookup"><span data-stu-id="af1cd-123">[Run the app in the Kudu console](#run-the-app-in-the-kudu-console) or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

<span data-ttu-id="af1cd-124">**連接重設**</span><span class="sxs-lookup"><span data-stu-id="af1cd-124">**Connection reset**</span></span>

<span data-ttu-id="af1cd-125">如果會傳送標頭之後，就會發生錯誤，則晚伺服器傳送**500 內部伺服器錯誤**發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="af1cd-125">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="af1cd-126">這通常發生在回應複雜物件的序列化期間發生錯誤時。</span><span class="sxs-lookup"><span data-stu-id="af1cd-126">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="af1cd-127">這種類型的錯誤會顯示為*連接重設*用戶端上的錯誤。</span><span class="sxs-lookup"><span data-stu-id="af1cd-127">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="af1cd-128">[應用程式記錄](xref:fundamentals/logging/index)可協助疑難排解這些類型的錯誤。</span><span class="sxs-lookup"><span data-stu-id="af1cd-128">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="af1cd-129">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="af1cd-129">Default startup limits</span></span>

<span data-ttu-id="af1cd-130">預設值設定 ASP.NET 核心模組*startupTimeLimit*為 120 秒。</span><span class="sxs-lookup"><span data-stu-id="af1cd-130">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="af1cd-131">當保留預設值，應用程式可能需要兩分鐘的時間啟動之前模組記錄處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-131">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="af1cd-132">如需設定此模組的資訊，請參閱[aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="af1cd-132">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="af1cd-133">疑難排解應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="af1cd-133">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="af1cd-134">應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="af1cd-134">Application Event Log</span></span>

<span data-ttu-id="af1cd-135">若要存取應用程式事件記錄檔，請使用**診斷並解決問題**刀鋒視窗，在 Azure 入口網站：</span><span class="sxs-lookup"><span data-stu-id="af1cd-135">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal :</span></span>

1. <span data-ttu-id="af1cd-136">在 Azure 入口網站中，開啟 [應用程式] 刀鋒視窗中的**應用程式服務**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-136">In the Azure portal, open the app's blade in the **App Services** blade.</span></span>
1. <span data-ttu-id="af1cd-137">選取**診斷並解決問題**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-137">Select the **Diagnose and solve problems** blade.</span></span>
1. <span data-ttu-id="af1cd-138">在下**選取問題類別**，選取**Web 應用程式向** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af1cd-138">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="af1cd-139">在下**建議的解決方案**，開啟的窗格**開啟應用程式事件記錄檔**。</span><span class="sxs-lookup"><span data-stu-id="af1cd-139">Under **Suggested Solutions**, open the pane for **Open Application Event Logs**.</span></span> <span data-ttu-id="af1cd-140">選取**開啟應用程式事件記錄檔** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af1cd-140">Select the **Open Application Event Logs** button.</span></span>
1. <span data-ttu-id="af1cd-141">檢查所提供的最新錯誤*IIS AspNetCoreModule*中**來源**資料行。</span><span class="sxs-lookup"><span data-stu-id="af1cd-141">Examine the latest error provided by the *IIS AspNetCoreModule* in the **Source** column.</span></span>

<span data-ttu-id="af1cd-142">除了使用**診斷並解決問題**刀鋒視窗會檢查直接使用的應用程式事件記錄檔[Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="af1cd-142">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="af1cd-143">選取**進階工具**刀鋒視窗中的**開發工具**區域。</span><span class="sxs-lookup"><span data-stu-id="af1cd-143">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="af1cd-144">選取**移&rarr;**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="af1cd-144">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="af1cd-145">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="af1cd-145">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="af1cd-146">使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。</span><span class="sxs-lookup"><span data-stu-id="af1cd-146">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="af1cd-147">開啟**LogFiles**資料夾。</span><span class="sxs-lookup"><span data-stu-id="af1cd-147">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="af1cd-148">選取鉛筆圖示旁*eventlog.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="af1cd-148">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="af1cd-149">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="af1cd-149">Examine the log.</span></span> <span data-ttu-id="af1cd-150">捲動至底部，以查看最新的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="af1cd-150">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="af1cd-151">Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="af1cd-151">Run the app in the Kudu console</span></span>

<span data-ttu-id="af1cd-152">許多的啟動錯誤不會產生應用程式事件日誌中的有用資訊。</span><span class="sxs-lookup"><span data-stu-id="af1cd-152">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="af1cd-153">您可以在執行應用程式[Kudu](https://github.com/projectkudu/kudu/wiki)發現錯誤的遠端執行主控台：</span><span class="sxs-lookup"><span data-stu-id="af1cd-153">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="af1cd-154">選取**進階工具**刀鋒視窗中的**開發工具**區域。</span><span class="sxs-lookup"><span data-stu-id="af1cd-154">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="af1cd-155">選取**移&rarr;**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="af1cd-155">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="af1cd-156">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="af1cd-156">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="af1cd-157">使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。</span><span class="sxs-lookup"><span data-stu-id="af1cd-157">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="af1cd-158">開啟資料夾的路徑**網站** > **wwwroot**。</span><span class="sxs-lookup"><span data-stu-id="af1cd-158">Open the folders to the path **site** > **wwwroot**.</span></span>
1. <span data-ttu-id="af1cd-159">在主控台中，執行應用程式藉由執行應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="af1cd-159">In the console, run the app by executing the app's assembly.</span></span>
   * <span data-ttu-id="af1cd-160">如果應用程式是[framework 相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，執行應用程式的組件*dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="af1cd-160">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), run the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="af1cd-161">下列命令，以取代應用程式的組件名稱`<assembly_name>`: `dotnet .\<assembly_name>.dll`</span><span class="sxs-lookup"><span data-stu-id="af1cd-161">In the following command, substitute the name of the app's assembly for `<assembly_name>`: `dotnet .\<assembly_name>.dll`</span></span>
   * <span data-ttu-id="af1cd-162">如果應用程式是[獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd)，請執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="af1cd-162">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd), run the app's executable.</span></span> <span data-ttu-id="af1cd-163">下列命令，以取代應用程式的組件名稱`<assembly_name>`: `<assembly_name>.exe`</span><span class="sxs-lookup"><span data-stu-id="af1cd-163">In the following command, substitute the name of the app's assembly for `<assembly_name>`: `<assembly_name>.exe`</span></span>
1. <span data-ttu-id="af1cd-164">主控台應用程式，並顯示任何錯誤，從輸出送到 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="af1cd-164">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="af1cd-165">ASP.NET 核心模組 stdout 記錄檔</span><span class="sxs-lookup"><span data-stu-id="af1cd-165">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="af1cd-166">ASP.NET 核心模組 stdout 記錄通常會記錄應用程式事件日誌中找不到有用的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="af1cd-166">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="af1cd-167">若要啟用，並檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="af1cd-167">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="af1cd-168">瀏覽至**診斷並解決問題**在 Azure 入口網站的刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-168">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="af1cd-169">在下**選取問題類別**，選取**Web 應用程式向** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af1cd-169">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="af1cd-170">在下**建議的解決方案** > **啟用 Stdout 記錄檔重新導向**，選取按鈕以**開啟 Kudu 主控台編輯 Web.Config**。</span><span class="sxs-lookup"><span data-stu-id="af1cd-170">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="af1cd-171">在 Kudu**診斷主控台**，開啟的資料夾路徑**網站** > **wwwroot**。</span><span class="sxs-lookup"><span data-stu-id="af1cd-171">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="af1cd-172">向下捲動至顯示*web.config*檔案清單的底部。</span><span class="sxs-lookup"><span data-stu-id="af1cd-172">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="af1cd-173">按一下鉛筆圖示旁*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="af1cd-173">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="af1cd-174">設定**stdoutLogEnabled**至`true`並變更**stdoutLogFile**路徑： `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="af1cd-174">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="af1cd-175">選取**儲存**來儲存已更新*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="af1cd-175">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="af1cd-176">應用程式提出要求。</span><span class="sxs-lookup"><span data-stu-id="af1cd-176">Make a request to the app.</span></span>
1. <span data-ttu-id="af1cd-177">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="af1cd-177">Return to the Azure portal.</span></span> <span data-ttu-id="af1cd-178">選取**進階工具**刀鋒視窗中的**開發工具**區域。</span><span class="sxs-lookup"><span data-stu-id="af1cd-178">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="af1cd-179">選取**移&rarr;**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="af1cd-179">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="af1cd-180">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="af1cd-180">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="af1cd-181">使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。</span><span class="sxs-lookup"><span data-stu-id="af1cd-181">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="af1cd-182">選取**LogFiles**資料夾。</span><span class="sxs-lookup"><span data-stu-id="af1cd-182">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="af1cd-183">檢查**Modified**資料行，然後選取鉛筆圖示，以編輯 stdout 記錄的最新的修改日期。</span><span class="sxs-lookup"><span data-stu-id="af1cd-183">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="af1cd-184">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="af1cd-184">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="af1cd-185">**重要！**</span><span class="sxs-lookup"><span data-stu-id="af1cd-185">**Important!**</span></span> <span data-ttu-id="af1cd-186">停用 stdout 記錄完成疑難排解時。</span><span class="sxs-lookup"><span data-stu-id="af1cd-186">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="af1cd-187">在 Kudu**診斷主控台**，返回路徑**網站** > **wwwroot**以顯示*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="af1cd-187">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="af1cd-188">開啟**web.config**檔案一次選取鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="af1cd-188">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="af1cd-189">設定**stdoutLogEnabled**至`false`。</span><span class="sxs-lookup"><span data-stu-id="af1cd-189">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="af1cd-190">選取**儲存**儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="af1cd-190">Select **Save** to save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="af1cd-191">若要停用 stdout 記錄的失敗可能會導致應用程式或伺服器失敗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-191">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="af1cd-192">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="af1cd-192">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="af1cd-193">例行記錄中的 ASP.NET Core 應用程式、 使用限制記錄檔大小，並且會旋轉記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="af1cd-193">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="af1cd-194">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="af1cd-194">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="af1cd-195">常見的啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="af1cd-195">Common startup errors</span></span> 

<span data-ttu-id="af1cd-196">請參閱[ASP.NET Core 常見錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)。</span><span class="sxs-lookup"><span data-stu-id="af1cd-196">See the [ASP.NET Core common errors reference](xref:host-and-deploy/azure-iis-errors-reference).</span></span> <span data-ttu-id="af1cd-197">參考主題會說明最常見的問題阻礙應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="af1cd-197">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="af1cd-198">較慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="af1cd-198">Slow or hanging app</span></span>

<span data-ttu-id="af1cd-199">當應用程式回應變慢或停止回應的要求時，請參閱[疑難排解緩慢的 web 應用程式在 Azure App Service 中的效能問題](/azure/app-service/app-service-web-troubleshoot-performance-degradation)偵錯指引。</span><span class="sxs-lookup"><span data-stu-id="af1cd-199">When an app responds slowly or hangs on a request, see [Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) for debugging guidance.</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="af1cd-200">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="af1cd-200">Remote debugging</span></span>

<span data-ttu-id="af1cd-201">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="af1cd-201">See the following topics:</span></span>

* <span data-ttu-id="af1cd-202">[遠端偵錯 web 應用程式的疑難排解中使用 Visual Studio 的 Azure App Service web 應用程式 區段](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)（Azure 文件）</span><span class="sxs-lookup"><span data-stu-id="af1cd-202">[Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure documentation)</span></span>
* <span data-ttu-id="af1cd-203">[遠端偵錯 ASP.NET Core 上 IIS 中的 Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) （Visual Studio 文件）</span><span class="sxs-lookup"><span data-stu-id="af1cd-203">[Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (Visual Studio documentation)</span></span>

## <a name="application-insights"></a><span data-ttu-id="af1cd-204">Application Insights</span><span class="sxs-lookup"><span data-stu-id="af1cd-204">Application Insights</span></span>

<span data-ttu-id="af1cd-205">[Application Insights](https://azure.microsoft.com/services/application-insights/)提供裝載於 Azure 應用程式服務，包括錯誤記錄和報告功能的應用程式的遙測。</span><span class="sxs-lookup"><span data-stu-id="af1cd-205">[Application Insights](https://azure.microsoft.com/services/application-insights/) provides telemetry from apps hosted in the Azure App Service, including error logging and reporting features.</span></span> <span data-ttu-id="af1cd-206">Application Insights 只可以在應用程式啟動時的應用程式記錄功能會變成可用之後，會發生的錯誤報告。</span><span class="sxs-lookup"><span data-stu-id="af1cd-206">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="af1cd-207">如需詳細資訊，請參閱[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="af1cd-207">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="monitoring-blades"></a><span data-ttu-id="af1cd-208">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="af1cd-208">Monitoring blades</span></span>

<span data-ttu-id="af1cd-209">監視刀鋒提供疑難排解體驗，本主題稍早所述之方法的替代方案。</span><span class="sxs-lookup"><span data-stu-id="af1cd-209">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="af1cd-210">可用來診斷 500 系列錯誤設定了這些刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-210">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="af1cd-211">確認已安裝 ASP.NET Core 擴充功能。</span><span class="sxs-lookup"><span data-stu-id="af1cd-211">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="af1cd-212">如果尚未安裝擴充功能，請手動進行安裝：</span><span class="sxs-lookup"><span data-stu-id="af1cd-212">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="af1cd-213">在**開發工具**刀鋒視窗區段中，選取**延伸**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-213">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="af1cd-214">**ASP.NET Core Extensions**應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="af1cd-214">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="af1cd-215">如果尚未安裝擴充功能，請選取**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="af1cd-215">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="af1cd-216">選擇**ASP.NET Core Extensions**從清單中。</span><span class="sxs-lookup"><span data-stu-id="af1cd-216">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="af1cd-217">選取**確定**接受法律合約。</span><span class="sxs-lookup"><span data-stu-id="af1cd-217">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="af1cd-218">選取**確定**上**將延伸加入**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-218">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="af1cd-219">資訊的快顯訊息表示已成功安裝擴充功能時。</span><span class="sxs-lookup"><span data-stu-id="af1cd-219">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="af1cd-220">如果未啟用 stdout 記錄，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="af1cd-220">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="af1cd-221">在 Azure 入口網站中，選取**進階工具**刀鋒視窗中的**開發工具**區域。</span><span class="sxs-lookup"><span data-stu-id="af1cd-221">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="af1cd-222">選取**移&rarr;**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="af1cd-222">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="af1cd-223">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="af1cd-223">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="af1cd-224">使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。</span><span class="sxs-lookup"><span data-stu-id="af1cd-224">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="af1cd-225">開啟資料夾的路徑**網站** > **wwwroot**和向下捲動至顯示*web.config*檔案清單的底部。</span><span class="sxs-lookup"><span data-stu-id="af1cd-225">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="af1cd-226">按一下鉛筆圖示旁*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="af1cd-226">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="af1cd-227">設定**stdoutLogEnabled**至`true`並變更**stdoutLogFile**路徑： `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="af1cd-227">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="af1cd-228">選取**儲存**來儲存已更新*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="af1cd-228">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="af1cd-229">若要啟用診斷記錄，繼續進行：</span><span class="sxs-lookup"><span data-stu-id="af1cd-229">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="af1cd-230">在 Azure 入口網站中，選取**診斷記錄檔**刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-230">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="af1cd-231">選取**上**切換**的應用程式記錄 （檔案系統）**和**詳細錯誤訊息**。</span><span class="sxs-lookup"><span data-stu-id="af1cd-231">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="af1cd-232">選取**儲存**刀鋒視窗頂端的按鈕。</span><span class="sxs-lookup"><span data-stu-id="af1cd-232">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="af1cd-233">若要包含失敗的要求追蹤，也就是失敗的要求事件緩衝處理 (FREB) 記錄中，選取**上**切換**追蹤失敗的要求**。</span><span class="sxs-lookup"><span data-stu-id="af1cd-233">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span> 
1. <span data-ttu-id="af1cd-234">選取**記錄檔資料流**刀鋒視窗，立即會列在底下**診斷記錄檔**在入口網站的刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-234">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="af1cd-235">應用程式提出要求。</span><span class="sxs-lookup"><span data-stu-id="af1cd-235">Make a request to the app.</span></span>
1. <span data-ttu-id="af1cd-236">在記錄檔之資料流資料，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="af1cd-236">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="af1cd-237">**重要！**</span><span class="sxs-lookup"><span data-stu-id="af1cd-237">**Important!**</span></span> <span data-ttu-id="af1cd-238">請務必停用 stdout 記錄完成疑難排解時。</span><span class="sxs-lookup"><span data-stu-id="af1cd-238">Be sure to disable stdout logging when troubleshooting is complete.</span></span> <span data-ttu-id="af1cd-239">中的指示，請參閱 < [ASP.NET 核心模組 stdout 記錄](#aspnet-core-module-stdout-log)> 一節。</span><span class="sxs-lookup"><span data-stu-id="af1cd-239">See the instructions in the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) section.</span></span>

<span data-ttu-id="af1cd-240">若要檢視失敗的要求追蹤記錄檔 （FREB 記錄檔）：</span><span class="sxs-lookup"><span data-stu-id="af1cd-240">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="af1cd-241">瀏覽至**診斷並解決問題**在 Azure 入口網站的刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-241">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="af1cd-242">選取**失敗的要求追蹤記錄**從**支援工具**區域的 [資訊看板]。</span><span class="sxs-lookup"><span data-stu-id="af1cd-242">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="af1cd-243">請參閱[失敗要求追蹤的 Azure App Service 主題中的 web 應用程式啟用診斷記錄 區段](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[在 Azure 中的 Web 應用程式的應用程式效能常見問題集： 如何開啟追蹤失敗的要求？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="af1cd-243">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="af1cd-244">如需詳細資訊，請參閱[啟用 Azure App Service 中的 web 應用程式的診斷記錄](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="af1cd-244">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="af1cd-245">若要停用 stdout 記錄的失敗可能會導致應用程式或伺服器失敗。</span><span class="sxs-lookup"><span data-stu-id="af1cd-245">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="af1cd-246">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="af1cd-246">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="af1cd-247">例行記錄中的 ASP.NET Core 應用程式、 使用限制記錄檔大小，並且會旋轉記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="af1cd-247">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="af1cd-248">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="af1cd-248">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="af1cd-249">其他資源</span><span class="sxs-lookup"><span data-stu-id="af1cd-249">Additional resources</span></span>

* [<span data-ttu-id="af1cd-250">ASP.NET Core 中的錯誤處理簡介</span><span class="sxs-lookup"><span data-stu-id="af1cd-250">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)
* [<span data-ttu-id="af1cd-251">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="af1cd-251">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="af1cd-252">疑難排解 Azure App Service 使用 Visual Studio 中的 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="af1cd-252">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="af1cd-253">疑難排解 「 502 不正確的閘道 」 和 「 503 服務無法使用 「 Azure web 應用程式中的 HTTP 錯誤</span><span class="sxs-lookup"><span data-stu-id="af1cd-253">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="af1cd-254">Azure App Service 中的速度慢的 web 應用程式效能問題的疑難排解</span><span class="sxs-lookup"><span data-stu-id="af1cd-254">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="af1cd-255">在 Azure 中的 Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="af1cd-255">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="af1cd-256">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="af1cd-256">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
