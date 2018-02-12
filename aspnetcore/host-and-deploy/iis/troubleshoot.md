---
title: "疑難排解在 IIS 上的 ASP.NET Core"
author: guardrex
description: "了解如何診斷問題的網際網路資訊服務 (IIS) 的 ASP.NET Core 應用程式的部署。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 65173e0101a17c64f4cde583e5bbb9fb0a9c7718
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="33d00-103">疑難排解在 IIS 上的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33d00-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="33d00-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="33d00-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="33d00-105">本文章提供指示如何診斷 ASP.NET Core 應用程式啟動問題時裝載具有[網際網路資訊服務 (IIS)](/iis)。</span><span class="sxs-lookup"><span data-stu-id="33d00-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="33d00-106">本文章中的資訊適用於裝載在 IIS 上 Windows Server 和 Windows 桌面。</span><span class="sxs-lookup"><span data-stu-id="33d00-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

<span data-ttu-id="33d00-107">在 Visual Studio 中的 ASP.NET Core 專案預設值為[IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)裝載在偵錯期間。</span><span class="sxs-lookup"><span data-stu-id="33d00-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="33d00-108">A *502.5 的處理序失敗*，發生於在本機偵錯可能會 troubleshooted 使用本主題中的建議。</span><span class="sxs-lookup"><span data-stu-id="33d00-108">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

<span data-ttu-id="33d00-109">其他的疑難排解主題：</span><span class="sxs-lookup"><span data-stu-id="33d00-109">Additional troubleshooting topics:</span></span>

[<span data-ttu-id="33d00-110">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="33d00-110">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="33d00-111">雖然應用程式服務會使用[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)和 IIS 主控件應用程式，請參閱應用程式服務的特定指示的專用的主題。</span><span class="sxs-lookup"><span data-stu-id="33d00-111">Although App Service uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

[<span data-ttu-id="33d00-112">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="33d00-112">Error handling</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="33d00-113">了解如何在本機系統上的開發期間處理 ASP.NET Core 應用程式中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="33d00-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="33d00-114">了解使用 Visual Studio 進行偵錯</span><span class="sxs-lookup"><span data-stu-id="33d00-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="33d00-115">本主題將介紹 Visual Studio 偵錯工具的功能。</span><span class="sxs-lookup"><span data-stu-id="33d00-115">This topic introduces the features of the Visual Studio debugger.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="33d00-116">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="33d00-116">App startup errors</span></span>

<span data-ttu-id="33d00-117">**502.5 處理程序失敗**</span><span class="sxs-lookup"><span data-stu-id="33d00-117">**502.5 Process Failure**</span></span>  
<span data-ttu-id="33d00-118">工作者處理序會失敗。</span><span class="sxs-lookup"><span data-stu-id="33d00-118">The worker process fails.</span></span> <span data-ttu-id="33d00-119">無法啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="33d00-119">The app doesn't start.</span></span>

<span data-ttu-id="33d00-120">ASP.NET 核心模組嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="33d00-120">The ASP.NET Core Module attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="33d00-121">處理序啟動失敗的原因通常可由中的項目[應用程式事件記錄檔](#application-event-log)和[ASP.NET 核心模組 stdout 記錄](#aspnet-core-module-stdout-log)。</span><span class="sxs-lookup"><span data-stu-id="33d00-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="33d00-122">*502.5 的處理序失敗*裝載或應用程式設定不正確而造成工作者處理序失敗時傳回錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="33d00-122">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![瀏覽器視窗中顯示 502.5 處理序失敗的頁面](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="33d00-124">**500 內部伺服器錯誤**</span><span class="sxs-lookup"><span data-stu-id="33d00-124">**500 Internal Server Error**</span></span>  
<span data-ttu-id="33d00-125">應用程式啟動，但因為錯誤而造成伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="33d00-125">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="33d00-126">在啟動期間，或建立回應時，應用程式的程式碼中會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="33d00-126">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="33d00-127">回應可能會包含任何內容，或回應可能會顯示為*500 內部伺服器錯誤*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="33d00-127">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="33d00-128">應用程式事件記錄檔通常會指出應用程式正常啟動。</span><span class="sxs-lookup"><span data-stu-id="33d00-128">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="33d00-129">從伺服器的觀點來看，這是正確。</span><span class="sxs-lookup"><span data-stu-id="33d00-129">From the server's perspective, that's correct.</span></span> <span data-ttu-id="33d00-130">應用程式沒有開始，但它無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="33d00-130">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="33d00-131">[在命令提示字元中執行應用程式](#run-the-app-at-a-command-prompt)在伺服器上或[啟用 ASP.NET 核心模組 stdout 記錄](#aspnet-core-module-stdout-log)來疑難排解問題。</span><span class="sxs-lookup"><span data-stu-id="33d00-131">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

<span data-ttu-id="33d00-132">**連接重設**</span><span class="sxs-lookup"><span data-stu-id="33d00-132">**Connection reset**</span></span>

<span data-ttu-id="33d00-133">如果會傳送標頭之後，就會發生錯誤，則晚伺服器傳送**500 內部伺服器錯誤**發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="33d00-133">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="33d00-134">這通常發生在回應複雜物件的序列化期間發生錯誤時。</span><span class="sxs-lookup"><span data-stu-id="33d00-134">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="33d00-135">這種類型的錯誤會顯示為*連接重設*用戶端上的錯誤。</span><span class="sxs-lookup"><span data-stu-id="33d00-135">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="33d00-136">[應用程式記錄](xref:fundamentals/logging/index)可協助疑難排解這些類型的錯誤。</span><span class="sxs-lookup"><span data-stu-id="33d00-136">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="33d00-137">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="33d00-137">Default startup limits</span></span>

<span data-ttu-id="33d00-138">預設值設定 ASP.NET 核心模組*startupTimeLimit*為 120 秒。</span><span class="sxs-lookup"><span data-stu-id="33d00-138">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="33d00-139">當保留預設值，應用程式可能需要兩分鐘的時間啟動之前模組記錄處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="33d00-139">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="33d00-140">如需設定此模組的資訊，請參閱[aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="33d00-140">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="33d00-141">疑難排解應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="33d00-141">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="33d00-142">應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="33d00-142">Application Event Log</span></span>

<span data-ttu-id="33d00-143">存取應用程式事件記錄檔：</span><span class="sxs-lookup"><span data-stu-id="33d00-143">Access the Application Event Log:</span></span>

1. <span data-ttu-id="33d00-144">開啟 [開始] 功能表中，搜尋**事件檢視器**，然後選取**事件檢視器**應用程式。</span><span class="sxs-lookup"><span data-stu-id="33d00-144">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="33d00-145">在**事件檢視器**，開啟**Windows 記錄檔**節點。</span><span class="sxs-lookup"><span data-stu-id="33d00-145">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="33d00-146">選取**應用程式**開啟應用程式事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="33d00-146">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="33d00-147">搜尋失敗應用程式相關聯的錯誤。</span><span class="sxs-lookup"><span data-stu-id="33d00-147">Search for errors associated with the failing app.</span></span> <span data-ttu-id="33d00-148">錯誤上面會有值為*IIS AspNetCore 模組*或*IIS Express AspNetCore 模組*中*來源*資料行。</span><span class="sxs-lookup"><span data-stu-id="33d00-148">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="33d00-149">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="33d00-149">Run the app at a command prompt</span></span>

<span data-ttu-id="33d00-150">許多的啟動錯誤不會產生應用程式事件日誌中的有用資訊。</span><span class="sxs-lookup"><span data-stu-id="33d00-150">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="33d00-151">您可以在命令提示字元中執行應用程式，在主機系統上，以找出某些錯誤原因。</span><span class="sxs-lookup"><span data-stu-id="33d00-151">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

<span data-ttu-id="33d00-152">**Framework 相依的部署**</span><span class="sxs-lookup"><span data-stu-id="33d00-152">**Framework-dependent deployment**</span></span>

<span data-ttu-id="33d00-153">如果應用程式是[framework 相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="33d00-153">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="33d00-154">在命令提示字元中，巡覽至部署資料夾並執行應用程式藉由執行應用程式的組件*dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="33d00-154">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="33d00-155">下列命令，以取代應用程式的組件名稱\<assembly_name >: `dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="33d00-155">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="33d00-156">主控台從應用程式，並顯示任何錯誤，輸出會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="33d00-156">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="33d00-157">如果應用程式提出要求時，就會發生錯誤，，提出要求的主機和 Kestrel 位置接聽的連接埠。</span><span class="sxs-lookup"><span data-stu-id="33d00-157">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="33d00-158">使用預設主控件和 post 要求提出要求來`http://localhost:5000/`。</span><span class="sxs-lookup"><span data-stu-id="33d00-158">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="33d00-159">如果應用程式通常回應 Kestrel 端點位址，此問題會更相關的反向 proxy 設定和比較不可能是應用程式內。</span><span class="sxs-lookup"><span data-stu-id="33d00-159">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

<span data-ttu-id="33d00-160">**獨立的部署**</span><span class="sxs-lookup"><span data-stu-id="33d00-160">**Self-contained deployment**</span></span>

<span data-ttu-id="33d00-161">如果應用程式是[獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="33d00-161">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="33d00-162">在命令提示字元中，瀏覽至部署資料夾，並執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="33d00-162">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="33d00-163">下列命令，以取代應用程式的組件名稱\<assembly_name >: `<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="33d00-163">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="33d00-164">主控台從應用程式，並顯示任何錯誤，輸出會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="33d00-164">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="33d00-165">如果應用程式提出要求時，就會發生錯誤，，提出要求的主機和 Kestrel 位置接聽的連接埠。</span><span class="sxs-lookup"><span data-stu-id="33d00-165">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="33d00-166">使用預設主控件和 post 要求提出要求來`http://localhost:5000/`。</span><span class="sxs-lookup"><span data-stu-id="33d00-166">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="33d00-167">如果應用程式通常回應 Kestrel 端點位址，此問題會更相關的反向 proxy 設定和比較不可能是應用程式內。</span><span class="sxs-lookup"><span data-stu-id="33d00-167">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="33d00-168">ASP.NET 核心模組 stdout 記錄檔</span><span class="sxs-lookup"><span data-stu-id="33d00-168">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="33d00-169">若要啟用，並檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="33d00-169">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="33d00-170">瀏覽至主機系統上的站台的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="33d00-170">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="33d00-171">如果*記錄*資料夾不存在、 建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="33d00-171">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="33d00-172">如需如何啟用 MSBuild 的指示來建立*記錄*部署中的資料夾自動執行，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="33d00-172">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="33d00-173">編輯*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="33d00-173">Edit the *web.config* file.</span></span> <span data-ttu-id="33d00-174">設定**stdoutLogEnabled**至`true`並變更**stdoutLogFile**路徑以指向*記錄檔*資料夾 (例如， `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="33d00-174">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="33d00-175">`stdout`在路徑中會記錄檔案名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="33d00-175">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="33d00-176">時間戳記、 處理序識別碼，以及檔案延伸模組會自動加入時建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="33d00-176">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="33d00-177">使用`stdout`一般記錄檔檔案名稱前置詞，以名為*stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="33d00-177">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="33d00-178">儲存已更新*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="33d00-178">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="33d00-179">應用程式提出要求。</span><span class="sxs-lookup"><span data-stu-id="33d00-179">Make a request to the app.</span></span>
1. <span data-ttu-id="33d00-180">瀏覽至*記錄*資料夾。</span><span class="sxs-lookup"><span data-stu-id="33d00-180">Navigate to the *logs* folder.</span></span> <span data-ttu-id="33d00-181">尋找並開啟最近 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="33d00-181">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="33d00-182">研究記錄的錯誤。</span><span class="sxs-lookup"><span data-stu-id="33d00-182">Study the log for errors.</span></span>

<span data-ttu-id="33d00-183">**重要 ！**</span><span class="sxs-lookup"><span data-stu-id="33d00-183">**Important!**</span></span> <span data-ttu-id="33d00-184">停用 stdout 記錄完成疑難排解時。</span><span class="sxs-lookup"><span data-stu-id="33d00-184">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="33d00-185">編輯*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="33d00-185">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="33d00-186">設定**stdoutLogEnabled**至`false`。</span><span class="sxs-lookup"><span data-stu-id="33d00-186">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="33d00-187">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="33d00-187">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="33d00-188">若要停用 stdout 記錄的失敗可能會導致應用程式或伺服器失敗。</span><span class="sxs-lookup"><span data-stu-id="33d00-188">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="33d00-189">沒有記錄檔的大小沒有限制或建立的記錄檔的數目。</span><span class="sxs-lookup"><span data-stu-id="33d00-189">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="33d00-190">例行記錄中的 ASP.NET Core 應用程式、 使用限制記錄檔大小，並且會旋轉記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="33d00-190">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="33d00-191">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="33d00-191">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enabling-the-developer-exception-page"></a><span data-ttu-id="33d00-192">啟用 [開發人員的例外狀況] 頁面</span><span class="sxs-lookup"><span data-stu-id="33d00-192">Enabling the Developer Exception Page</span></span>

<span data-ttu-id="33d00-193">`ASPNETCORE_ENVIRONMENT` [環境變數可以加入至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)開發環境中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="33d00-193">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="33d00-194">只要環境不覆寫中的應用程式啟動`UseEnvironment`主控件產生器中，設定環境變數可讓[開發人員例外狀況頁面](xref:fundamentals/error-handling)出現應用程式執行時。</span><span class="sxs-lookup"><span data-stu-id="33d00-194">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="33d00-195">設定環境變數，如`ASPNETCORE_ENVIRONMENT`只建議用於籌畫與測試不會被公開到網際網路的伺服器上使用。</span><span class="sxs-lookup"><span data-stu-id="33d00-195">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="33d00-196">移除環境變數從*web.config*疑難排解後的檔案。</span><span class="sxs-lookup"><span data-stu-id="33d00-196">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="33d00-197">如需有關設定環境變數詳細*web.config*，請參閱[aspNetCore environmentVariables 子項目](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="33d00-197">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="33d00-198">常見的啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="33d00-198">Common startup errors</span></span> 

<span data-ttu-id="33d00-199">請參閱[ASP.NET Core 常見錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)。</span><span class="sxs-lookup"><span data-stu-id="33d00-199">See the [ASP.NET Core common errors reference](xref:host-and-deploy/azure-iis-errors-reference).</span></span> <span data-ttu-id="33d00-200">參考主題會說明最常見的問題阻礙應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="33d00-200">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="33d00-201">較慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="33d00-201">Slow or hanging app</span></span>

<span data-ttu-id="33d00-202">當應用程式回應變慢或停止回應的要求時，取得並分析[傾印檔案](/visualstudio/debugger/using-dump-files)。</span><span class="sxs-lookup"><span data-stu-id="33d00-202">When an app responds slowly or hangs on a request, obtain and analyze a [dump file](/visualstudio/debugger/using-dump-files).</span></span> <span data-ttu-id="33d00-203">您可以使用任何下列工具取得傾印檔案：</span><span class="sxs-lookup"><span data-stu-id="33d00-203">Dump files can be obtained using any of the following tools:</span></span>

* [<span data-ttu-id="33d00-204">ProcDump</span><span class="sxs-lookup"><span data-stu-id="33d00-204">ProcDump</span></span>](/sysinternals/downloads/procdump)
* [<span data-ttu-id="33d00-205">DebugDiag</span><span class="sxs-lookup"><span data-stu-id="33d00-205">DebugDiag</span></span>](https://www.microsoft.com/download/details.aspx?id=49924)
* <span data-ttu-id="33d00-206">WinDbg:[下載適用於 Windows 的偵錯工具](https://developer.microsoft.com/windows/hardware/download-windbg)，[偵錯使用 WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span><span class="sxs-lookup"><span data-stu-id="33d00-206">WinDbg: [Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="33d00-207">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="33d00-207">Remote debugging</span></span>

<span data-ttu-id="33d00-208">請參閱[遠端偵錯 ASP.NET Core 上遠端 IIS 電腦在 Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio 文件中。</span><span class="sxs-lookup"><span data-stu-id="33d00-208">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="33d00-209">Application Insights</span><span class="sxs-lookup"><span data-stu-id="33d00-209">Application Insights</span></span>

<span data-ttu-id="33d00-210">[Application Insights](/azure/application-insights/)提供從裝載的 IIS，包括錯誤記錄和報告功能的應用程式的遙測。</span><span class="sxs-lookup"><span data-stu-id="33d00-210">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="33d00-211">Application Insights 只可以在應用程式啟動時的應用程式記錄功能會變成可用之後，會發生的錯誤報告。</span><span class="sxs-lookup"><span data-stu-id="33d00-211">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="33d00-212">如需詳細資訊，請參閱[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="33d00-212">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-troubleshooting-advice"></a><span data-ttu-id="33d00-213">其他疑難排解建議</span><span class="sxs-lookup"><span data-stu-id="33d00-213">Additional troubleshooting advice</span></span>

<span data-ttu-id="33d00-214">有時正常運作的應用程式後，無法立即升級其中一個.NET Core SDK 在應用程式內的開發電腦或套件版本。</span><span class="sxs-lookup"><span data-stu-id="33d00-214">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="33d00-215">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="33d00-215">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="33d00-216">遵循下列指示，就可以修正問題：</span><span class="sxs-lookup"><span data-stu-id="33d00-216">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="33d00-217">刪除*bin*和*obj*資料夾。</span><span class="sxs-lookup"><span data-stu-id="33d00-217">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="33d00-218">封裝在快取的清除*%userprofile%\\.nuget\\封裝*和*%localappdata%\\Nuget\\v3 快取*。</span><span class="sxs-lookup"><span data-stu-id="33d00-218">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="33d00-219">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="33d00-219">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="33d00-220">請確認先前的部署在伺服器上有之前重新部署應用程式完全刪除。</span><span class="sxs-lookup"><span data-stu-id="33d00-220">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="33d00-221">清除封裝快取的便利方式是執行`dotnet nuget locals all --clear`從命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="33d00-221">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
> 
> <span data-ttu-id="33d00-222">清除封裝快取也可藉由使用[nuget.exe](https://www.nuget.org/downloads)工具並執行命令`nuget locals all -clear`。</span><span class="sxs-lookup"><span data-stu-id="33d00-222">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="33d00-223">*nuget.exe*未在 Windows 桌面作業系統配套的安裝，且必須取自分別[NuGet 網站](https://www.nuget.org/downloads)。</span><span class="sxs-lookup"><span data-stu-id="33d00-223">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="33d00-224">其他資源</span><span class="sxs-lookup"><span data-stu-id="33d00-224">Additional resources</span></span>

* [<span data-ttu-id="33d00-225">ASP.NET Core 中的錯誤處理簡介</span><span class="sxs-lookup"><span data-stu-id="33d00-225">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)
* [<span data-ttu-id="33d00-226">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="33d00-226">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="33d00-227">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="33d00-227">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="33d00-228">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="33d00-228">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
