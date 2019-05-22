---
title: 針對 IIS 上的 ASP.NET Core 進行疑難排解
author: guardrex
description: 了解如何診斷 ASP.NET Core 應用程式的 Internet Information Services (IIS) 部署問題。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 80994cb84e9e0658ee90198b6bf992e5b374bf3c
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970025"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="31917-103">針對 IIS 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="31917-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="31917-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="31917-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="31917-105">本文說明以 [Internet Information Services (IIS)](/iis) 裝載 ASP.NET Core 應用程式時，如何診斷此應用程式的啟動問題。</span><span class="sxs-lookup"><span data-stu-id="31917-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="31917-106">本文中資訊適用的裝載環境是 Windows Server 和 Windows 桌面上的 IIS。</span><span class="sxs-lookup"><span data-stu-id="31917-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="31917-107">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="31917-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="31917-108">在本機偵錯時發生的「502.5 - 處理序失敗」或「500.30 - 啟動失敗」可以使用本主題中的建議在該位置進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="31917-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="31917-109">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="31917-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="31917-110">在本機偵錯時發生的「502.5 處理序失敗」可以使用本主題中的建議來進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="31917-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="31917-111">其他疑難排解主題：</span><span class="sxs-lookup"><span data-stu-id="31917-111">Additional troubleshooting topics:</span></span>

<span data-ttu-id="31917-112"><xref:host-and-deploy/azure-apps/troubleshoot> 雖然 App Service 使用 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)和 IIS 來裝載應用程式，但如需 App Service 特定的指示，請參閱其專屬的主題。</span><span class="sxs-lookup"><span data-stu-id="31917-112"><xref:host-and-deploy/azure-apps/troubleshoot> Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<span data-ttu-id="31917-113"><xref:fundamentals/error-handling> 了解在本機系統上進行開發時，如何處理 ASP.NET Core 應用程式中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="31917-113"><xref:fundamentals/error-handling> Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

<span data-ttu-id="31917-114">[了解如何使用 Visual Studio 進行偵錯](/visualstudio/debugger/getting-started-with-the-debugger) 本主題介紹 Visual Studio 偵錯工具的功能。</span><span class="sxs-lookup"><span data-stu-id="31917-114">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="31917-115">[使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\) 了解 Visual Studio Code 中內建的偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="31917-115">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="31917-116">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="31917-116">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="31917-117">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="31917-117">502.5 Process Failure</span></span>

<span data-ttu-id="31917-118">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="31917-118">The worker process fails.</span></span> <span data-ttu-id="31917-119">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="31917-119">The app doesn't start.</span></span>

<span data-ttu-id="31917-120">ASP.NET Core 模組嘗試啟動後端 dotnet 處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="31917-120">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="31917-121">通常從[應用程式事件記錄檔](#application-event-log)和 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="31917-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="31917-122">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="31917-122">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="31917-123">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="31917-123">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="31917-124">「共用的架構」是一組安裝於電腦上且由 `Microsoft.AspNetCore.App` 之類的中繼套件所參考的組件 (*.dll* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="31917-124">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="31917-125">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="31917-125">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="31917-126">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="31917-126">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="31917-127">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗] 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="31917-127">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![顯示 [502.5 處理序失敗] 頁面的瀏覽器視窗](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="31917-129">500.30 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="31917-129">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="31917-130">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="31917-130">The worker process fails.</span></span> <span data-ttu-id="31917-131">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="31917-131">The app doesn't start.</span></span>

<span data-ttu-id="31917-132">ASP.NET Core 模組嘗試啟動 .NET Core CLR 同處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="31917-132">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="31917-133">通常從[應用程式事件記錄檔](#application-event-log)和 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="31917-133">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="31917-134">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="31917-134">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="31917-135">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="31917-135">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="31917-136">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="31917-136">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="31917-137">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="31917-137">The worker process fails.</span></span> <span data-ttu-id="31917-138">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="31917-138">The app doesn't start.</span></span>

<span data-ttu-id="31917-139">The ASP.NET Core 模組找不到 .NET Core CLR，並尋找同處理序要求處理常式 (*aspnetcorev2_inprocess.dll*)。</span><span class="sxs-lookup"><span data-stu-id="31917-139">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="31917-140">請檢查︰</span><span class="sxs-lookup"><span data-stu-id="31917-140">Check that:</span></span>

* <span data-ttu-id="31917-141">應用程式以 [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet 套件或 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)為目標。</span><span class="sxs-lookup"><span data-stu-id="31917-141">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="31917-142">應用程式設為目標的 ASP.NET Core 共用架構版本有安裝在目標機器上。</span><span class="sxs-lookup"><span data-stu-id="31917-142">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="31917-143">500.0 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="31917-143">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="31917-144">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="31917-144">The worker process fails.</span></span> <span data-ttu-id="31917-145">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="31917-145">The app doesn't start.</span></span>

<span data-ttu-id="31917-146">ASP.NET Core 模組找不到跨處理序裝載要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="31917-146">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="31917-147">請確定 *aspnetcorev2_outofprocess.dll* 出現在子資料夾中，且位於 *aspnetcorev2.dll* 旁。</span><span class="sxs-lookup"><span data-stu-id="31917-147">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="31917-148">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="31917-148">500 Internal Server Error</span></span>

<span data-ttu-id="31917-149">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="31917-149">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="31917-150">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="31917-150">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="31917-151">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」的形式出現。</span><span class="sxs-lookup"><span data-stu-id="31917-151">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="31917-152">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="31917-152">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="31917-153">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="31917-153">From the server's perspective, that's correct.</span></span> <span data-ttu-id="31917-154">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="31917-154">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="31917-155">請在伺服器上[於命令提示字元中執行應用程式](#run-the-app-at-a-command-prompt)或[啟用 ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="31917-155">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="31917-156">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="31917-156">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="31917-157">應用程式無法啟動，因為無法載入應用程式的組件 (*.dll*)。</span><span class="sxs-lookup"><span data-stu-id="31917-157">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="31917-158">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="31917-158">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="31917-159">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="31917-159">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="31917-160">在 IIS 管理員的 [應用程式集區] 中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="31917-160">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="31917-161">在 [動作] 面板中，選取 [編輯應用程式集區] 下的 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="31917-161">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="31917-162">設定 [啟用 32 位元應用程式]：</span><span class="sxs-lookup"><span data-stu-id="31917-162">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="31917-163">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="31917-163">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="31917-164">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="31917-164">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="31917-165">連線重設</span><span class="sxs-lookup"><span data-stu-id="31917-165">Connection reset</span></span>

<span data-ttu-id="31917-166">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="31917-166">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="31917-167">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="31917-167">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="31917-168">這類錯誤會在用戶端上顯示為「連線重設」錯誤。</span><span class="sxs-lookup"><span data-stu-id="31917-168">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="31917-169">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="31917-169">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="31917-170">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="31917-170">Default startup limits</span></span>

<span data-ttu-id="31917-171">ASP.NET Core 模組上已設定預設的 *startupTimeLimit* 120 秒。</span><span class="sxs-lookup"><span data-stu-id="31917-171">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="31917-172">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="31917-172">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="31917-173">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="31917-173">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="31917-174">針對應用程式啟動錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="31917-174">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="31917-175">啟用 ASP.NET Core 模組偵錯記錄</span><span class="sxs-lookup"><span data-stu-id="31917-175">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="31917-176">將下列處理常式設定新增至應用程式的 *web.config* 檔案，以啟用 ASP.NET Core 模組偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="31917-176">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="31917-177">確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="31917-177">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="31917-178">應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="31917-178">Application Event Log</span></span>

<span data-ttu-id="31917-179">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="31917-179">Access the Application Event Log:</span></span>

1. <span data-ttu-id="31917-180">開啟 [開始] 功能表，搜尋**事件檢視器**，然後選取 [事件檢視器]應用程式。</span><span class="sxs-lookup"><span data-stu-id="31917-180">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="31917-181">在 [事件檢視器] 中，開啟 [Windows 記錄] 節點。</span><span class="sxs-lookup"><span data-stu-id="31917-181">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="31917-182">選取 [應用程式] 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="31917-182">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="31917-183">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="31917-183">Search for errors associated with the failing app.</span></span> <span data-ttu-id="31917-184">錯誤在 [來源] 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="31917-184">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="31917-185">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="31917-185">Run the app at a command prompt</span></span>

<span data-ttu-id="31917-186">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="31917-186">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="31917-187">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="31917-187">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="31917-188">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="31917-188">Framework-dependent deployment</span></span>

<span data-ttu-id="31917-189">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="31917-189">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="31917-190">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="31917-190">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="31917-191">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="31917-191">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="31917-192">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="31917-192">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="31917-193">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="31917-193">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="31917-194">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="31917-194">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="31917-195">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="31917-195">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="31917-196">自封式部署</span><span class="sxs-lookup"><span data-stu-id="31917-196">Self-contained deployment</span></span>

<span data-ttu-id="31917-197">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="31917-197">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="31917-198">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="31917-198">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="31917-199">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="31917-199">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="31917-200">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="31917-200">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="31917-201">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="31917-201">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="31917-202">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="31917-202">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="31917-203">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="31917-203">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="31917-204">ASP.NET Core 模組 stdout 記錄檔</span><span class="sxs-lookup"><span data-stu-id="31917-204">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="31917-205">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="31917-205">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="31917-206">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="31917-206">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="31917-207">如果 [logs] 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="31917-207">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="31917-208">如需有關如何讓 MSBuild 在部署中自動建立 [logs] 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="31917-208">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="31917-209">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="31917-209">Edit the *web.config* file.</span></span> <span data-ttu-id="31917-210">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs] 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="31917-210">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="31917-211">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="31917-211">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="31917-212">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="31917-212">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="31917-213">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="31917-213">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="31917-214">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="31917-214">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="31917-215">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="31917-215">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="31917-216">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="31917-216">Make a request to the app.</span></span>
1. <span data-ttu-id="31917-217">瀏覽至 [logs] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="31917-217">Navigate to the *logs* folder.</span></span> <span data-ttu-id="31917-218">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="31917-218">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="31917-219">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="31917-219">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31917-220">完成疑難排解時，請停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="31917-220">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="31917-221">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="31917-221">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="31917-222">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="31917-222">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="31917-223">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="31917-223">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="31917-224">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="31917-224">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="31917-225">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="31917-225">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="31917-226">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="31917-226">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="31917-227">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="31917-227">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="31917-228">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="31917-228">Enable the Developer Exception Page</span></span>

<span data-ttu-id="31917-229">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="31917-229">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="31917-230">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="31917-230">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="31917-231">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="31917-231">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="31917-232">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="31917-232">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="31917-233">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="31917-233">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="31917-234">常見的啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="31917-234">Common startup errors</span></span>

<span data-ttu-id="31917-235">請參閱 <xref:host-and-deploy/azure-iis-errors-reference>。</span><span class="sxs-lookup"><span data-stu-id="31917-235">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="31917-236">參考主題涵蓋了大多數導致應用程式無法啟動的常見問題。</span><span class="sxs-lookup"><span data-stu-id="31917-236">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="31917-237">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="31917-237">Obtain data from an app</span></span>

<span data-ttu-id="31917-238">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="31917-238">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="31917-239">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="31917-239">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="31917-240">建立傾印</span><span class="sxs-lookup"><span data-stu-id="31917-240">Create a dump</span></span>

<span data-ttu-id="31917-241">「傾印」是系統記憶體的快照集，可協助您判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="31917-241">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="31917-242">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="31917-242">App crashes or encounters an exception</span></span>

<span data-ttu-id="31917-243">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="31917-243">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="31917-244">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="31917-244">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="31917-245">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="31917-245">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="31917-246">執行 [EnableDumps PowerShell 指令碼](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="31917-246">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="31917-247">如果應用程式是使用[同處理序主控模型](xref:fundamentals/servers/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="31917-247">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="31917-248">如果應用程式是使用[跨處理序主控模型](xref:fundamentals/servers/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="31917-248">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="31917-249">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="31917-249">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="31917-250">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="31917-250">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="31917-251">如果應用程式是使用[同處理序主控模型](xref:fundamentals/servers/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="31917-251">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="31917-252">如果應用程式是使用[跨處理序主控模型](xref:fundamentals/servers/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="31917-252">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="31917-253">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="31917-253">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="31917-254">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="31917-254">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="31917-255">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="31917-255">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="31917-256">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="31917-256">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="31917-257">當應用程式「停止回應」(停止回應但未損毀)、在啟動期間失敗，或正常執行時，請參閱[使用者模式傾印檔案：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)，選取適當的工具來產生傾印。</span><span class="sxs-lookup"><span data-stu-id="31917-257">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="31917-258">分析傾印</span><span class="sxs-lookup"><span data-stu-id="31917-258">Analyze the dump</span></span>

<span data-ttu-id="31917-259">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="31917-259">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="31917-260">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="31917-260">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="31917-261">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="31917-261">Remote debugging</span></span>

<span data-ttu-id="31917-262">請參閱 Visual Studio 文件中的[在 Visual Studio 2017 中針對遠端 IIS 電腦上的 ASP.NET Core 進行遠端偵錯](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) \(機器翻譯\)。</span><span class="sxs-lookup"><span data-stu-id="31917-262">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="31917-263">Application Insights</span><span class="sxs-lookup"><span data-stu-id="31917-263">Application Insights</span></span>

<span data-ttu-id="31917-264">[Application Insights](/azure/application-insights/) 提供來自 IIS 所裝載應用程式的遙測資料，包括錯誤記錄和報告功能。</span><span class="sxs-lookup"><span data-stu-id="31917-264">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="31917-265">Application Insights 只能針對在應用程式啟動之後，應用程式記錄功能變成可用時所發生的錯誤提出報告。</span><span class="sxs-lookup"><span data-stu-id="31917-265">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="31917-266">如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="31917-266">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="31917-267">額外建議</span><span class="sxs-lookup"><span data-stu-id="31917-267">Additional advice</span></span>

<span data-ttu-id="31917-268">有時，在升級開發電腦上的 .NET Core SDK 或應用程式內的套件版本之後，正常運作的應用程式便立即發生失敗。</span><span class="sxs-lookup"><span data-stu-id="31917-268">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="31917-269">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="31917-269">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="31917-270">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="31917-270">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="31917-271">刪除 [bin] 和 [obj] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="31917-271">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="31917-272">清除位於 *%UserProfile%\\.nuget\\packages* 和 *%LocalAppData%\\Nuget\\v3-cache*的套件快取。</span><span class="sxs-lookup"><span data-stu-id="31917-272">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="31917-273">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="31917-273">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="31917-274">先確認已將伺服器上先前的部署完全刪除，再重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="31917-274">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="31917-275">有一個便利的方式可以清除套件快取，就是從命令提示字元執行 `dotnet nuget locals all --clear`。</span><span class="sxs-lookup"><span data-stu-id="31917-275">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="31917-276">您也可以使用 [nuget.exe](https://www.nuget.org/downloads) 工具並執行 `nuget locals all -clear` 命令，來清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="31917-276">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="31917-277">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="31917-277">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31917-278">其他資源</span><span class="sxs-lookup"><span data-stu-id="31917-278">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
