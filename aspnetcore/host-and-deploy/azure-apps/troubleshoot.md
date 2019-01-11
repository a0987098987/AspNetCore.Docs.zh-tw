---
title: 針對 Azure App Service 上的 ASP.NET Core 啟動錯誤進行疑難排解
author: guardrex
description: 了解如何診斷 ASP.NET Core Azure App Service 部署的問題。
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: b36c321c6ba6801a32b5187651063337b4533fd1
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637647"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a><span data-ttu-id="c833c-103">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="c833c-103">Troubleshoot ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="c833c-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c833c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="c833c-105">本文說明如何使用 Azure App Service 診斷工具來診斷 ASP.NET Core 應用程式啟動問題。</span><span class="sxs-lookup"><span data-stu-id="c833c-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue using Azure App Service's diagnostic tools.</span></span> <span data-ttu-id="c833c-106">如需其他疑難排解建議，請參閱 Azure 文件中的 [Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)和[如何：監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)。</span><span class="sxs-lookup"><span data-stu-id="c833c-106">For additional troubleshooting advice, see [Azure App Service diagnostics overview](/azure/app-service/app-service-diagnostics) and [How to: Monitor Apps in Azure App Service](/azure/app-service/web-sites-monitor) in the Azure documentation.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="c833c-107">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="c833c-107">App startup errors</span></span>

<span data-ttu-id="c833c-108">**502.5 處理序失敗**</span><span class="sxs-lookup"><span data-stu-id="c833c-108">**502.5 Process Failure**</span></span>  
<span data-ttu-id="c833c-109">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="c833c-109">The worker process fails.</span></span> <span data-ttu-id="c833c-110">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="c833c-110">The app doesn't start.</span></span>

<span data-ttu-id="c833c-111">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="c833c-111">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="c833c-112">檢查「應用程式事件記錄檔」通常有助於針對這類問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="c833c-112">Examining the Application Event Log often helps troubleshoot this type of problem.</span></span> <span data-ttu-id="c833c-113">[應用程式事件記錄檔](#application-event-log)一節說明了如何存取此記錄檔。</span><span class="sxs-lookup"><span data-stu-id="c833c-113">Accessing the log is explained in the [Application Event Log](#application-event-log) section.</span></span>

<span data-ttu-id="c833c-114">當設定錯誤的應用程式造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗] 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="c833c-114">The *502.5 Process Failure* error page is returned when a misconfigured app causes the worker process to fail:</span></span>

![顯示 [502.5 處理序失敗] 頁面的瀏覽器視窗](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="c833c-116">**500 內部伺服器錯誤**</span><span class="sxs-lookup"><span data-stu-id="c833c-116">**500 Internal Server Error**</span></span>  
<span data-ttu-id="c833c-117">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="c833c-117">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="c833c-118">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="c833c-118">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="c833c-119">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」的形式出現。</span><span class="sxs-lookup"><span data-stu-id="c833c-119">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="c833c-120">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="c833c-120">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="c833c-121">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="c833c-121">From the server's perspective, that's correct.</span></span> <span data-ttu-id="c833c-122">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="c833c-122">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="c833c-123">請[在 Kudu 主控台中執行應用程式](#run-the-app-in-the-kudu-console)或[啟用 ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="c833c-123">[Run the app in the Kudu console](#run-the-app-in-the-kudu-console) or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

<span data-ttu-id="c833c-124">**連線重設**</span><span class="sxs-lookup"><span data-stu-id="c833c-124">**Connection reset**</span></span>

<span data-ttu-id="c833c-125">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="c833c-125">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="c833c-126">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="c833c-126">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="c833c-127">這類錯誤會在用戶端上顯示為「連線重設」錯誤。</span><span class="sxs-lookup"><span data-stu-id="c833c-127">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="c833c-128">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="c833c-128">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="c833c-129">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="c833c-129">Default startup limits</span></span>

<span data-ttu-id="c833c-130">ASP.NET Core 模組上已設定預設的 *startupTimeLimit* 120 秒。</span><span class="sxs-lookup"><span data-stu-id="c833c-130">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="c833c-131">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="c833c-131">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="c833c-132">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="c833c-132">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="c833c-133">針對應用程式啟動錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="c833c-133">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="c833c-134">應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="c833c-134">Application Event Log</span></span>

<span data-ttu-id="c833c-135">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題] 刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="c833c-135">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal :</span></span>

1. <span data-ttu-id="c833c-136">在 Azure 入口網站的 [應用程式服務] 刀鋒視窗中，開啟應用程式的刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c833c-136">In the Azure portal, open the app's blade in the **App Services** blade.</span></span>
1. <span data-ttu-id="c833c-137">選取 [診斷並解決問題] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c833c-137">Select the **Diagnose and solve problems** blade.</span></span>
1. <span data-ttu-id="c833c-138">在 [選取問題類別] 底下，選取 [Web 應用程式停止運作] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c833c-138">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="c833c-139">在 [建議的解決方案] 底下，開啟用來**開啟應用程式事件記錄檔**的窗格。</span><span class="sxs-lookup"><span data-stu-id="c833c-139">Under **Suggested Solutions**, open the pane for **Open Application Event Logs**.</span></span> <span data-ttu-id="c833c-140">選取 [開啟應用程式事件記錄檔] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c833c-140">Select the **Open Application Event Logs** button.</span></span>
1. <span data-ttu-id="c833c-141">檢查 [來源] 資料行中 *IIS AspNetCoreModule* 所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="c833c-141">Examine the latest error provided by the *IIS AspNetCoreModule* in the **Source** column.</span></span>

<span data-ttu-id="c833c-142">除了使用 [診斷並解決問題] 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="c833c-142">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="c833c-143">選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c833c-143">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="c833c-144">選取 [執行&rarr;] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c833c-144">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="c833c-145">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="c833c-145">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="c833c-146">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="c833c-146">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="c833c-147">開啟 [LogFiles] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c833c-147">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="c833c-148">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="c833c-148">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="c833c-149">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="c833c-149">Examine the log.</span></span> <span data-ttu-id="c833c-150">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="c833c-150">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="c833c-151">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="c833c-151">Run the app in the Kudu console</span></span>

<span data-ttu-id="c833c-152">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="c833c-152">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="c833c-153">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="c833c-153">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="c833c-154">選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c833c-154">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="c833c-155">選取 [執行&rarr;] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c833c-155">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="c833c-156">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="c833c-156">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="c833c-157">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="c833c-157">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="c833c-158">將資料夾開啟至路徑 [site] > [wwwroot]。</span><span class="sxs-lookup"><span data-stu-id="c833c-158">Open the folders to the path **site** > **wwwroot**.</span></span>
1. <span data-ttu-id="c833c-159">在主控台中，藉由執行應用程式的組件來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c833c-159">In the console, run the app by executing the app's assembly.</span></span>
   * <span data-ttu-id="c833c-160">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請使用 *dotnet.exe* 來執行應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="c833c-160">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), run the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="c833c-161">在下列命令中，請以應用程式組件的名稱取代 `<assembly_name>`：`dotnet .\<assembly_name>.dll`</span><span class="sxs-lookup"><span data-stu-id="c833c-161">In the following command, substitute the name of the app's assembly for `<assembly_name>`: `dotnet .\<assembly_name>.dll`</span></span>
   * <span data-ttu-id="c833c-162">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，請執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="c833c-162">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd), run the app's executable.</span></span> <span data-ttu-id="c833c-163">在下列命令中，請以應用程式組件的名稱取代 `<assembly_name>`：`<assembly_name>.exe`</span><span class="sxs-lookup"><span data-stu-id="c833c-163">In the following command, substitute the name of the app's assembly for `<assembly_name>`: `<assembly_name>.exe`</span></span>
1. <span data-ttu-id="c833c-164">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="c833c-164">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="c833c-165">ASP.NET Core 模組 stdout 記錄檔</span><span class="sxs-lookup"><span data-stu-id="c833c-165">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="c833c-166">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="c833c-166">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="c833c-167">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="c833c-167">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="c833c-168">在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c833c-168">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="c833c-169">在 [選取問題類別] 底下，選取 [Web 應用程式停止運作] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c833c-169">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="c833c-170">在 [建議的解決方案] > [啟用 Stdout 記錄檔重新導向] 底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="c833c-170">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="c833c-171">在 Kudu [診斷主控台] 中，將資料夾開啟至路徑 [site] > [wwwroot]。</span><span class="sxs-lookup"><span data-stu-id="c833c-171">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="c833c-172">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c833c-172">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="c833c-173">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="c833c-173">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="c833c-174">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="c833c-174">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="c833c-175">選取 [儲存] 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c833c-175">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="c833c-176">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="c833c-176">Make a request to the app.</span></span>
1. <span data-ttu-id="c833c-177">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="c833c-177">Return to the Azure portal.</span></span> <span data-ttu-id="c833c-178">選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c833c-178">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="c833c-179">選取 [執行&rarr;] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c833c-179">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="c833c-180">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="c833c-180">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="c833c-181">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="c833c-181">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="c833c-182">選取 [LogFiles] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c833c-182">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="c833c-183">檢查 [修改時間] 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="c833c-183">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="c833c-184">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="c833c-184">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="c833c-185">**重要！**</span><span class="sxs-lookup"><span data-stu-id="c833c-185">**Important!**</span></span> <span data-ttu-id="c833c-186">完成疑難排解時，請停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="c833c-186">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="c833c-187">在 Kudu [診斷主控台] 中，返回路徑 [site] > [wwwroot] 以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c833c-187">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="c833c-188">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="c833c-188">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="c833c-189">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="c833c-189">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="c833c-190">選取 [儲存] 以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="c833c-190">Select **Save** to save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="c833c-191">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="c833c-191">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="c833c-192">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="c833c-192">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="c833c-193">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="c833c-193">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="c833c-194">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="c833c-194">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="c833c-195">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="c833c-195">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="c833c-196">常見的啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="c833c-196">Common startup errors</span></span> 

<span data-ttu-id="c833c-197">請參閱 <xref:host-and-deploy/azure-iis-errors-reference>。</span><span class="sxs-lookup"><span data-stu-id="c833c-197">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="c833c-198">參考主題涵蓋了大多數導致應用程式無法啟動的常見問題。</span><span class="sxs-lookup"><span data-stu-id="c833c-198">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="c833c-199">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="c833c-199">Slow or hanging app</span></span>

<span data-ttu-id="c833c-200">當應用程式針對要求回應緩慢或無回應時，請參閱[針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解](/azure/app-service/app-service-web-troubleshoot-performance-degradation)來取得疑難排解指南。</span><span class="sxs-lookup"><span data-stu-id="c833c-200">When an app responds slowly or hangs on a request, see [Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) for debugging guidance.</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="c833c-201">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="c833c-201">Remote debugging</span></span>

<span data-ttu-id="c833c-202">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="c833c-202">See the following topics:</span></span>

* <span data-ttu-id="c833c-203">[＜使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式＞的＜遠端偵錯 Web 應用程式＞一節](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure 文件)</span><span class="sxs-lookup"><span data-stu-id="c833c-203">[Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure documentation)</span></span>
* <span data-ttu-id="c833c-204">[在 Visual Studio 2017 中針對 Azure 中 IIS 上的 ASP.NET Core 進行遠端偵錯](/visualstudio/debugger/remote-debugging-azure) \(機器翻譯\) (Visual Studio 文件)</span><span class="sxs-lookup"><span data-stu-id="c833c-204">[Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (Visual Studio documentation)</span></span>

## <a name="application-insights"></a><span data-ttu-id="c833c-205">Application Insights</span><span class="sxs-lookup"><span data-stu-id="c833c-205">Application Insights</span></span>

<span data-ttu-id="c833c-206">[Application Insights](https://azure.microsoft.com/services/application-insights/) 提供來自 Azure App Service 中所裝載應用程式的遙測資料，包括錯誤記錄和報告功能。</span><span class="sxs-lookup"><span data-stu-id="c833c-206">[Application Insights](https://azure.microsoft.com/services/application-insights/) provides telemetry from apps hosted in the Azure App Service, including error logging and reporting features.</span></span> <span data-ttu-id="c833c-207">Application Insights 只能針對在應用程式啟動之後，應用程式記錄功能變成可用時所發生的錯誤提出報告。</span><span class="sxs-lookup"><span data-stu-id="c833c-207">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="c833c-208">如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="c833c-208">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="monitoring-blades"></a><span data-ttu-id="c833c-209">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="c833c-209">Monitoring blades</span></span>

<span data-ttu-id="c833c-210">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="c833c-210">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="c833c-211">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="c833c-211">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="c833c-212">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="c833c-212">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="c833c-213">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="c833c-213">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="c833c-214">在 [開發工具] 刀鋒視窗區段中，選取 [延伸模組] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c833c-214">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="c833c-215">[ASP.NET Core 延伸模組] 應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="c833c-215">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="c833c-216">如果未安裝這些延伸模組，請選取 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c833c-216">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="c833c-217">從清單中選擇 [ASP.NET Core 延伸模組]。</span><span class="sxs-lookup"><span data-stu-id="c833c-217">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="c833c-218">選取 [確定] 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="c833c-218">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="c833c-219">在 [新增延伸模組] 刀鋒視窗上，選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="c833c-219">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="c833c-220">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="c833c-220">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="c833c-221">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="c833c-221">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="c833c-222">在 Azure 入口網站中，選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c833c-222">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="c833c-223">選取 [執行&rarr;] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c833c-223">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="c833c-224">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="c833c-224">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="c833c-225">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="c833c-225">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="c833c-226">將資料夾開啟至路徑 [site] > [wwwroot]，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c833c-226">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="c833c-227">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="c833c-227">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="c833c-228">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="c833c-228">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="c833c-229">選取 [儲存] 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c833c-229">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="c833c-230">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="c833c-230">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="c833c-231">在 Azure 入口網站中，選取 [診斷記錄檔] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c833c-231">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="c833c-232">選取 [應用程式記錄 (檔案系統)] 和 [詳細錯誤訊息].的 [開啟] 開關。</span><span class="sxs-lookup"><span data-stu-id="c833c-232">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="c833c-233">選取刀鋒視窗頂端的 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c833c-233">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="c833c-234">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤] 的 [開啟] 開關。</span><span class="sxs-lookup"><span data-stu-id="c833c-234">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span> 
1. <span data-ttu-id="c833c-235">選取 [記錄資料流] 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔] 刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="c833c-235">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="c833c-236">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="c833c-236">Make a request to the app.</span></span>
1. <span data-ttu-id="c833c-237">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="c833c-237">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="c833c-238">**重要！**</span><span class="sxs-lookup"><span data-stu-id="c833c-238">**Important!**</span></span> <span data-ttu-id="c833c-239">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="c833c-239">Be sure to disable stdout logging when troubleshooting is complete.</span></span> <span data-ttu-id="c833c-240">請參閱 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)一節中的指示。</span><span class="sxs-lookup"><span data-stu-id="c833c-240">See the instructions in the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) section.</span></span>

<span data-ttu-id="c833c-241">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="c833c-241">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="c833c-242">在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="c833c-242">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="c833c-243">從資訊看板的 [支援工具] 區域中，選取 [失敗要求追蹤記錄檔]。</span><span class="sxs-lookup"><span data-stu-id="c833c-243">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="c833c-244">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和 [Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="c833c-244">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="c833c-245">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="c833c-245">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="c833c-246">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="c833c-246">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="c833c-247">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="c833c-247">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="c833c-248">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="c833c-248">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="c833c-249">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="c833c-249">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c833c-250">其他資源</span><span class="sxs-lookup"><span data-stu-id="c833c-250">Additional resources</span></span>

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="c833c-251">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c833c-251">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="c833c-252">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="c833c-252">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="c833c-253">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="c833c-253">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="c833c-254">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="c833c-254">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="c833c-255">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="c833c-255">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="c833c-256">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="c833c-256">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
