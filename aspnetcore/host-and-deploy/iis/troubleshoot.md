---
title: 針對 IIS 上的 ASP.NET Core 進行疑難排解
author: guardrex
description: 了解如何診斷 ASP.NET Core 應用程式的 Internet Information Services (IIS) 部署問題。
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: d57196693feb6413560ec25e09cf74e9babf93bf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276161"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="5683c-103">針對 IIS 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="5683c-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="5683c-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5683c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5683c-105">本文說明以 [Internet Information Services (IIS)](/iis) 裝載 ASP.NET Core 應用程式時，如何診斷此應用程式的啟動問題。</span><span class="sxs-lookup"><span data-stu-id="5683c-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="5683c-106">本文中資訊適用的裝載環境是 Windows Server 和 Windows 桌面上的 IIS。</span><span class="sxs-lookup"><span data-stu-id="5683c-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

<span data-ttu-id="5683c-107">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="5683c-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="5683c-108">在本機偵錯時發生的「502.5 處理序失敗」可以使用本主題中的建議來進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="5683c-108">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

<span data-ttu-id="5683c-109">其他疑難排解主題：</span><span class="sxs-lookup"><span data-stu-id="5683c-109">Additional troubleshooting topics:</span></span>

[<span data-ttu-id="5683c-110">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="5683c-110">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="5683c-111">雖然 App Service 使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)和 IIS 來裝載應用程式，但如需 App Service 特定的指示，請參閱其專屬的主題。</span><span class="sxs-lookup"><span data-stu-id="5683c-111">Although App Service uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

[<span data-ttu-id="5683c-112">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="5683c-112">Handle errors</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="5683c-113">了解在本機系統上進行開發時，如何處理 ASP.NET Core 應用程式中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="5683c-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="5683c-114">了解使用 Visual Studio 進行偵錯</span><span class="sxs-lookup"><span data-stu-id="5683c-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="5683c-115">本主題將介紹 Visual Studio 偵錯工具的功能。</span><span class="sxs-lookup"><span data-stu-id="5683c-115">This topic introduces the features of the Visual Studio debugger.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="5683c-116">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="5683c-116">App startup errors</span></span>

<span data-ttu-id="5683c-117">**502.5 處理序失敗**</span><span class="sxs-lookup"><span data-stu-id="5683c-117">**502.5 Process Failure**</span></span>  
<span data-ttu-id="5683c-118">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="5683c-118">The worker process fails.</span></span> <span data-ttu-id="5683c-119">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="5683c-119">The app doesn't start.</span></span>

<span data-ttu-id="5683c-120">ASP.NET Core 模組嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="5683c-120">The ASP.NET Core Module attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="5683c-121">通常從[應用程式事件記錄檔](#application-event-log)和 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="5683c-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="5683c-122">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗] 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="5683c-122">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![顯示 [502.5 處理序失敗] 頁面的瀏覽器視窗](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="5683c-124">**500 內部伺服器錯誤**</span><span class="sxs-lookup"><span data-stu-id="5683c-124">**500 Internal Server Error**</span></span>  
<span data-ttu-id="5683c-125">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="5683c-125">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="5683c-126">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="5683c-126">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="5683c-127">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」的形式出現。</span><span class="sxs-lookup"><span data-stu-id="5683c-127">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="5683c-128">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="5683c-128">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="5683c-129">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="5683c-129">From the server's perspective, that's correct.</span></span> <span data-ttu-id="5683c-130">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="5683c-130">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="5683c-131">請在伺服器上[於命令提示字元中執行應用程式](#run-the-app-at-a-command-prompt)或[啟用 ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="5683c-131">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

<span data-ttu-id="5683c-132">**連線重設**</span><span class="sxs-lookup"><span data-stu-id="5683c-132">**Connection reset**</span></span>

<span data-ttu-id="5683c-133">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="5683c-133">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="5683c-134">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="5683c-134">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="5683c-135">這類錯誤會在用戶端上顯示為「連線重設」錯誤。</span><span class="sxs-lookup"><span data-stu-id="5683c-135">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="5683c-136">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="5683c-136">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="5683c-137">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="5683c-137">Default startup limits</span></span>

<span data-ttu-id="5683c-138">ASP.NET Core 模組上已設定預設的 *startupTimeLimit* 120 秒。</span><span class="sxs-lookup"><span data-stu-id="5683c-138">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="5683c-139">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="5683c-139">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="5683c-140">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="5683c-140">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="5683c-141">針對應用程式啟動錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="5683c-141">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="5683c-142">應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="5683c-142">Application Event Log</span></span>

<span data-ttu-id="5683c-143">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="5683c-143">Access the Application Event Log:</span></span>

1. <span data-ttu-id="5683c-144">開啟 [開始] 功能表，搜尋**事件檢視器**，然後選取 [事件檢視器]應用程式。</span><span class="sxs-lookup"><span data-stu-id="5683c-144">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="5683c-145">在 [事件檢視器] 中，開啟 [Windows 記錄] 節點。</span><span class="sxs-lookup"><span data-stu-id="5683c-145">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="5683c-146">選取 [應用程式] 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="5683c-146">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="5683c-147">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="5683c-147">Search for errors associated with the failing app.</span></span> <span data-ttu-id="5683c-148">錯誤在 [來源] 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="5683c-148">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="5683c-149">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="5683c-149">Run the app at a command prompt</span></span>

<span data-ttu-id="5683c-150">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="5683c-150">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="5683c-151">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="5683c-151">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

<span data-ttu-id="5683c-152">**架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="5683c-152">**Framework-dependent deployment**</span></span>

<span data-ttu-id="5683c-153">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="5683c-153">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="5683c-154">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5683c-154">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="5683c-155">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="5683c-155">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="5683c-156">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="5683c-156">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="5683c-157">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="5683c-157">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="5683c-158">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="5683c-158">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="5683c-159">如果應用程式在 Kestrel 端點位址正常回應，則問題與反向 Proxy 設定有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="5683c-159">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

<span data-ttu-id="5683c-160">**自封式部署**</span><span class="sxs-lookup"><span data-stu-id="5683c-160">**Self-contained deployment**</span></span>

<span data-ttu-id="5683c-161">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="5683c-161">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="5683c-162">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="5683c-162">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="5683c-163">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="5683c-163">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="5683c-164">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="5683c-164">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="5683c-165">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="5683c-165">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="5683c-166">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="5683c-166">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="5683c-167">如果應用程式在 Kestrel 端點位址正常回應，則問題與反向 Proxy 設定有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="5683c-167">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="5683c-168">ASP.NET Core 模組 stdout 記錄檔</span><span class="sxs-lookup"><span data-stu-id="5683c-168">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="5683c-169">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="5683c-169">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="5683c-170">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="5683c-170">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="5683c-171">如果 [logs] 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="5683c-171">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="5683c-172">如需有關如何讓 MSBuild 在部署中自動建立 [logs] 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="5683c-172">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="5683c-173">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5683c-173">Edit the *web.config* file.</span></span> <span data-ttu-id="5683c-174">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs] 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="5683c-174">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="5683c-175">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="5683c-175">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="5683c-176">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="5683c-176">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="5683c-177">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="5683c-177">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="5683c-178">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5683c-178">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="5683c-179">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="5683c-179">Make a request to the app.</span></span>
1. <span data-ttu-id="5683c-180">瀏覽至 [logs] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5683c-180">Navigate to the *logs* folder.</span></span> <span data-ttu-id="5683c-181">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="5683c-181">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="5683c-182">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="5683c-182">Study the log for errors.</span></span>

<span data-ttu-id="5683c-183">**重要！**</span><span class="sxs-lookup"><span data-stu-id="5683c-183">**Important!**</span></span> <span data-ttu-id="5683c-184">完成疑難排解時，請停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="5683c-184">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="5683c-185">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5683c-185">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="5683c-186">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="5683c-186">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="5683c-187">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="5683c-187">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="5683c-188">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="5683c-188">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="5683c-189">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="5683c-189">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="5683c-190">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="5683c-190">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="5683c-191">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="5683c-191">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enabling-the-developer-exception-page"></a><span data-ttu-id="5683c-192">啟用開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="5683c-192">Enabling the Developer Exception Page</span></span>

<span data-ttu-id="5683c-193">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5683c-193">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="5683c-194">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="5683c-194">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="5683c-195">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="5683c-195">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="5683c-196">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="5683c-196">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="5683c-197">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="5683c-197">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="5683c-198">常見的啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="5683c-198">Common startup errors</span></span> 

<span data-ttu-id="5683c-199">請參閱 [ASP.NET Core 常見錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)。</span><span class="sxs-lookup"><span data-stu-id="5683c-199">See the [ASP.NET Core common errors reference](xref:host-and-deploy/azure-iis-errors-reference).</span></span> <span data-ttu-id="5683c-200">參考主題涵蓋了大多數導致應用程式無法啟動的常見問題。</span><span class="sxs-lookup"><span data-stu-id="5683c-200">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="5683c-201">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="5683c-201">Slow or hanging app</span></span>

<span data-ttu-id="5683c-202">當應用程式針對要求回應緩慢或無回應時，請取得[傾印檔案](/visualstudio/debugger/using-dump-files)並加以分析。</span><span class="sxs-lookup"><span data-stu-id="5683c-202">When an app responds slowly or hangs on a request, obtain and analyze a [dump file](/visualstudio/debugger/using-dump-files).</span></span> <span data-ttu-id="5683c-203">您可以使用下列任何工具來取得傾印檔案：</span><span class="sxs-lookup"><span data-stu-id="5683c-203">Dump files can be obtained using any of the following tools:</span></span>

* [<span data-ttu-id="5683c-204">ProcDump</span><span class="sxs-lookup"><span data-stu-id="5683c-204">ProcDump</span></span>](/sysinternals/downloads/procdump)
* [<span data-ttu-id="5683c-205">DebugDiag</span><span class="sxs-lookup"><span data-stu-id="5683c-205">DebugDiag</span></span>](https://www.microsoft.com/download/details.aspx?id=49924)
* <span data-ttu-id="5683c-206">WinDbg：[下載適用於 Windows 的偵錯工具](https://developer.microsoft.com/windows/hardware/download-windbg)[使用 WinDbg 進行偵錯](/windows-hardware/drivers/debugger/debugging-using-windbg)</span><span class="sxs-lookup"><span data-stu-id="5683c-206">WinDbg: [Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="5683c-207">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="5683c-207">Remote debugging</span></span>

<span data-ttu-id="5683c-208">請參閱 Visual Studio 文件中的[在 Visual Studio 2017 中針對遠端 IIS 電腦上的 ASP.NET Core 進行遠端偵錯](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) \(機器翻譯\)。</span><span class="sxs-lookup"><span data-stu-id="5683c-208">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="5683c-209">Application Insights</span><span class="sxs-lookup"><span data-stu-id="5683c-209">Application Insights</span></span>

<span data-ttu-id="5683c-210">[Application Insights](/azure/application-insights/) 提供來自 IIS 所裝載應用程式的遙測資料，包括錯誤記錄和報告功能。</span><span class="sxs-lookup"><span data-stu-id="5683c-210">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="5683c-211">Application Insights 只能針對在應用程式啟動之後，應用程式記錄功能變成可用時所發生的錯誤提出報告。</span><span class="sxs-lookup"><span data-stu-id="5683c-211">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="5683c-212">如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="5683c-212">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-troubleshooting-advice"></a><span data-ttu-id="5683c-213">其他疑難排解建議</span><span class="sxs-lookup"><span data-stu-id="5683c-213">Additional troubleshooting advice</span></span>

<span data-ttu-id="5683c-214">有時，在升級開發電腦上的 .NET Core SDK 或應用程式內的套件版本之後，正常運作的應用程式便立即發生失敗。</span><span class="sxs-lookup"><span data-stu-id="5683c-214">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="5683c-215">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="5683c-215">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="5683c-216">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="5683c-216">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="5683c-217">刪除 [bin] 和 [obj] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5683c-217">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="5683c-218">清除位於 *%UserProfile%\\.nuget\\packages* 和 *%LocalAppData%\\Nuget\\v3-cache*的套件快取。</span><span class="sxs-lookup"><span data-stu-id="5683c-218">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="5683c-219">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="5683c-219">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="5683c-220">先確認已將伺服器上先前的部署完全刪除，再重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="5683c-220">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="5683c-221">有一個便利的方式可以清除套件快取，就是從命令提示字元執行 `dotnet nuget locals all --clear`。</span><span class="sxs-lookup"><span data-stu-id="5683c-221">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
> 
> <span data-ttu-id="5683c-222">您也可以使用 [nuget.exe](https://www.nuget.org/downloads) 工具並執行 `nuget locals all -clear` 命令，來清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="5683c-222">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="5683c-223">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="5683c-223">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5683c-224">其他資源</span><span class="sxs-lookup"><span data-stu-id="5683c-224">Additional resources</span></span>

* [<span data-ttu-id="5683c-225">ASP.NET Core 中的錯誤處理簡介</span><span class="sxs-lookup"><span data-stu-id="5683c-225">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)
* [<span data-ttu-id="5683c-226">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="5683c-226">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="5683c-227">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="5683c-227">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="5683c-228">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="5683c-228">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
