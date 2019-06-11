---
title: 針對 Azure App Service 上的 ASP.NET Core 進行疑難排解
author: guardrex
description: 了解如何診斷 ASP.NET Core Azure App Service 部署的問題。
ms.author: riande
ms.custom: mvc
ms.date: 03/06/2019
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 7a0bb7df27ebbea0eac79771452295846fad563a
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470450"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a><span data-ttu-id="f383e-103">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="f383e-103">Troubleshoot ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="f383e-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f383e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="f383e-105">本文說明如何使用 Azure App Service 診斷工具來診斷 ASP.NET Core 應用程式啟動問題。</span><span class="sxs-lookup"><span data-stu-id="f383e-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue using Azure App Service's diagnostic tools.</span></span> <span data-ttu-id="f383e-106">如需其他疑難排解建議，請參閱 Azure 文件中的 [Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)和[如何：監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)。</span><span class="sxs-lookup"><span data-stu-id="f383e-106">For additional troubleshooting advice, see [Azure App Service diagnostics overview](/azure/app-service/app-service-diagnostics) and [How to: Monitor Apps in Azure App Service](/azure/app-service/web-sites-monitor) in the Azure documentation.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="f383e-107">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="f383e-107">App startup errors</span></span>

<span data-ttu-id="f383e-108">**502.5 處理序失敗** 背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="f383e-108">**502.5 Process Failure** The worker process fails.</span></span> <span data-ttu-id="f383e-109">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-109">The app doesn't start.</span></span>

<span data-ttu-id="f383e-110">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-110">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="f383e-111">檢查「應用程式事件記錄檔」通常有助於針對這類問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="f383e-111">Examining the Application Event Log often helps troubleshoot this type of problem.</span></span> <span data-ttu-id="f383e-112">[應用程式事件記錄檔](#application-event-log)一節說明了如何存取此記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f383e-112">Accessing the log is explained in the [Application Event Log](#application-event-log) section.</span></span>

<span data-ttu-id="f383e-113">當設定錯誤的應用程式造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗]  錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="f383e-113">The *502.5 Process Failure* error page is returned when a misconfigured app causes the worker process to fail:</span></span>

![顯示 [502.5 處理序失敗] 頁面的瀏覽器視窗](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="f383e-115">**500 內部伺服器錯誤**</span><span class="sxs-lookup"><span data-stu-id="f383e-115">**500 Internal Server Error**</span></span>

<span data-ttu-id="f383e-116">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="f383e-116">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="f383e-117">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="f383e-117">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="f383e-118">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」  的形式出現。</span><span class="sxs-lookup"><span data-stu-id="f383e-118">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="f383e-119">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-119">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="f383e-120">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="f383e-120">From the server's perspective, that's correct.</span></span> <span data-ttu-id="f383e-121">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="f383e-121">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="f383e-122">請[在 Kudu 主控台中執行應用程式](#run-the-app-in-the-kudu-console)或[啟用 ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="f383e-122">[Run the app in the Kudu console](#run-the-app-in-the-kudu-console) or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="f383e-123">500.30 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="f383e-123">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="f383e-124">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="f383e-124">The worker process fails.</span></span> <span data-ttu-id="f383e-125">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-125">The app doesn't start.</span></span>

<span data-ttu-id="f383e-126">ASP.NET Core 模組嘗試啟動 .NET Core CLR 同處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-126">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="f383e-127">通常從[應用程式事件記錄檔](#application-event-log)和 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="f383e-127">The cause of a process startup failure is usually determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="f383e-128">500.31 ANCM 找不到原生相依性</span><span class="sxs-lookup"><span data-stu-id="f383e-128">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="f383e-129">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="f383e-129">The worker process fails.</span></span> <span data-ttu-id="f383e-130">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-130">The app doesn't start.</span></span>

<span data-ttu-id="f383e-131">ASP.NET Core 模組嘗試啟動 .NET Core 執行階段同處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-131">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="f383e-132">此啟動失敗的最常見原因是當 `Microsoft.NETCore.App` 或 `Microsoft.AspNetCore.App` 執行階段未安裝時。</span><span class="sxs-lookup"><span data-stu-id="f383e-132">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="f383e-133">如果應用程式部署至目標 ASP.NET Core 3.0，但電腦上無該版本，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="f383e-133">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="f383e-134">範例錯誤訊息如下：</span><span class="sxs-lookup"><span data-stu-id="f383e-134">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="f383e-135">錯誤訊息會列出所有已安裝 .NET Core 版本和應用程式所要求的版本。</span><span class="sxs-lookup"><span data-stu-id="f383e-135">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="f383e-136">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="f383e-136">To fix this error, either:</span></span>

* <span data-ttu-id="f383e-137">在電腦上安裝適當的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="f383e-137">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="f383e-138">將應用程式的目標 .NET Core 版本變更為電腦上版本。</span><span class="sxs-lookup"><span data-stu-id="f383e-138">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="f383e-139">將應用程式發佈為[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="f383e-139">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="f383e-140">在開發過程中執行時 (`ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`)，特定的錯誤會寫入至 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="f383e-140">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="f383e-141">處理序啟動失敗的原因也會列在[應用程式事件記錄檔](#application-event-log)中。</span><span class="sxs-lookup"><span data-stu-id="f383e-141">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="f383e-142">500.32 ANCM 無法載入 dll</span><span class="sxs-lookup"><span data-stu-id="f383e-142">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="f383e-143">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="f383e-143">The worker process fails.</span></span> <span data-ttu-id="f383e-144">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-144">The app doesn't start.</span></span>

<span data-ttu-id="f383e-145">此錯誤最常見原因是針對不相容的處理器架構發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="f383e-145">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="f383e-146">如果背景工作處理序執行為 32 位元應用程式，而此應用程式已發佈至目標 64 位元，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="f383e-146">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="f383e-147">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="f383e-147">To fix this error, either:</span></span>

* <span data-ttu-id="f383e-148">針對相同的處理器架構，將應用程式重新發佈為背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="f383e-148">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="f383e-149">將應用程式發佈為[架構相依部署](/dotnet/core/deploying/#framework-dependent-executables-fde)。</span><span class="sxs-lookup"><span data-stu-id="f383e-149">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="f383e-150">500.33 ANCM 要求處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="f383e-150">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="f383e-151">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="f383e-151">The worker process fails.</span></span> <span data-ttu-id="f383e-152">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-152">The app doesn't start.</span></span>

<span data-ttu-id="f383e-153">應用程式未參考 `Microsoft.AspNetCore.App` 架構。</span><span class="sxs-lookup"><span data-stu-id="f383e-153">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="f383e-154">ASP.NET Core 模組只裝載以 `Microsoft.AspNetCore.App` 架構為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f383e-154">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the ASP.NET Core Module.</span></span>

<span data-ttu-id="f383e-155">若要修正這個錯誤，請確認應用程式以 `Microsoft.AspNetCore.App` 架構為目標。</span><span class="sxs-lookup"><span data-stu-id="f383e-155">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="f383e-156">檢查 `.runtimeconfig.json` 以驗證應用程式是否以該架構為目標。</span><span class="sxs-lookup"><span data-stu-id="f383e-156">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="f383e-157">500.34 ANCM 不支援混合式裝載模型</span><span class="sxs-lookup"><span data-stu-id="f383e-157">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="f383e-158">背景工作處理序無法在相同的程序中執行同處理序應用程式和跨處理序應用程式。</span><span class="sxs-lookup"><span data-stu-id="f383e-158">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="f383e-159">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f383e-159">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="f383e-160">500.35 ANCM 同一程序中有多個同處理序應用程式</span><span class="sxs-lookup"><span data-stu-id="f383e-160">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="f383e-161">背景工作處理序無法在相同的程序中執行同處理序應用程式和跨處理序應用程式。</span><span class="sxs-lookup"><span data-stu-id="f383e-161">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="f383e-162">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f383e-162">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="f383e-163">500.36 ANCM 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="f383e-163">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="f383e-164">跨處理序要求處理常式 *aspnetcorev2_outofprocess.dll* 不在 *aspnetcorev2.dll* 檔案旁邊。</span><span class="sxs-lookup"><span data-stu-id="f383e-164">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="f383e-165">這表示 ASP.NET Core 模組安裝損毀。</span><span class="sxs-lookup"><span data-stu-id="f383e-165">This indicates a corrupted installation of the ASP.NET Core Module.</span></span>

<span data-ttu-id="f383e-166">若要修正這個錯誤，請修復 [.NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (適用於 IIS) 或 Visual Studio (適用於 IIS Express) 安裝。</span><span class="sxs-lookup"><span data-stu-id="f383e-166">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="f383e-167">500.37 ANCM 無法在啟動時間限制內啟動</span><span class="sxs-lookup"><span data-stu-id="f383e-167">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="f383e-168">ANCM 無法在提供的啟動時間限制內啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-168">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="f383e-169">根據預設，逾時值為 120 秒。</span><span class="sxs-lookup"><span data-stu-id="f383e-169">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="f383e-170">在同一部電腦上啟動大量的應用程式時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="f383e-170">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="f383e-171">檢查伺服器在啟動期間是否出現 CPU/記憶體的使用量尖峰。</span><span class="sxs-lookup"><span data-stu-id="f383e-171">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="f383e-172">多個應用程式的啟動程序可能需要交錯進行。</span><span class="sxs-lookup"><span data-stu-id="f383e-172">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="f383e-173">500.30 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="f383e-173">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="f383e-174">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="f383e-174">The worker process fails.</span></span> <span data-ttu-id="f383e-175">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-175">The app doesn't start.</span></span>

<span data-ttu-id="f383e-176">ASP.NET Core 模組嘗試啟動 .NET Core 執行階段同處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-176">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="f383e-177">通常從[應用程式事件記錄檔](#application-event-log)和 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="f383e-177">The cause of a process startup failure is usually determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="f383e-178">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="f383e-178">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="f383e-179">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="f383e-179">The worker process fails.</span></span> <span data-ttu-id="f383e-180">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-180">The app doesn't start.</span></span>

<span data-ttu-id="f383e-181">處理序啟動失敗的原因也會列在[應用程式事件記錄檔](#application-event-log)中。</span><span class="sxs-lookup"><span data-stu-id="f383e-181">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

::: moniker-end

<span data-ttu-id="f383e-182">**連線重設**</span><span class="sxs-lookup"><span data-stu-id="f383e-182">**Connection reset**</span></span>

<span data-ttu-id="f383e-183">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」  。</span><span class="sxs-lookup"><span data-stu-id="f383e-183">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="f383e-184">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="f383e-184">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="f383e-185">這類錯誤會在用戶端上顯示為「連線重設」  錯誤。</span><span class="sxs-lookup"><span data-stu-id="f383e-185">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="f383e-186">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="f383e-186">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="f383e-187">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="f383e-187">Default startup limits</span></span>

<span data-ttu-id="f383e-188">ASP.NET Core 模組上已設定預設的 *startupTimeLimit* 120 秒。</span><span class="sxs-lookup"><span data-stu-id="f383e-188">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="f383e-189">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="f383e-189">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="f383e-190">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="f383e-190">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="f383e-191">針對應用程式啟動錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="f383e-191">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="f383e-192">應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="f383e-192">Application Event Log</span></span>

<span data-ttu-id="f383e-193">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題]  刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="f383e-193">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="f383e-194">在 Azure 入口網站的 [應用程式服務]  中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="f383e-194">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="f383e-195">選取 [診斷並解決問題]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-195">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="f383e-196">選取 [診斷工具]  標題。</span><span class="sxs-lookup"><span data-stu-id="f383e-196">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="f383e-197">在 [支援工具]  下，選取 [應用程式事件]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-197">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="f383e-198">檢查 [來源]  資料行中 *IIS AspNetCoreModule* 或 *IIS AspNetCoreModule V2* 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="f383e-198">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="f383e-199">除了使用 [診斷並解決問題]  刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="f383e-199">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="f383e-200">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-200">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="f383e-201">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-201">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f383e-202">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="f383e-202">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="f383e-203">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-203">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="f383e-204">開啟 [LogFiles]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="f383e-204">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="f383e-205">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="f383e-205">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="f383e-206">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f383e-206">Examine the log.</span></span> <span data-ttu-id="f383e-207">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="f383e-207">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="f383e-208">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="f383e-208">Run the app in the Kudu console</span></span>

<span data-ttu-id="f383e-209">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="f383e-209">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="f383e-210">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="f383e-210">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="f383e-211">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-211">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="f383e-212">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-212">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f383e-213">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="f383e-213">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="f383e-214">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-214">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="f383e-215">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="f383e-215">Test a 32-bit (x86) app</span></span>

##### <a name="current-release"></a><span data-ttu-id="f383e-216">目前的版本</span><span class="sxs-lookup"><span data-stu-id="f383e-216">Current release</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="f383e-217">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="f383e-217">Run the app:</span></span>
   * <span data-ttu-id="f383e-218">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="f383e-218">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="f383e-219">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="f383e-219">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="f383e-220">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="f383e-220">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a><span data-ttu-id="f383e-221">在預覽版上執行的架構相依部署</span><span class="sxs-lookup"><span data-stu-id="f383e-221">Framework-dependent deployment running on a preview release</span></span>

<span data-ttu-id="f383e-222">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="f383e-222">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="f383e-223">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="f383e-223">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="f383e-224">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="f383e-224">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="f383e-225">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="f383e-225">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="f383e-226">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="f383e-226">Test a 64-bit (x64) app</span></span>

##### <a name="current-release"></a><span data-ttu-id="f383e-227">目前的版本</span><span class="sxs-lookup"><span data-stu-id="f383e-227">Current release</span></span>

* <span data-ttu-id="f383e-228">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="f383e-228">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="f383e-229">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="f383e-229">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="f383e-230">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="f383e-230">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="f383e-231">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="f383e-231">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="f383e-232">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="f383e-232">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a><span data-ttu-id="f383e-233">在預覽版上執行的架構相依部署</span><span class="sxs-lookup"><span data-stu-id="f383e-233">Framework-dependent deployment running on a preview release</span></span>

<span data-ttu-id="f383e-234">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="f383e-234">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="f383e-235">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="f383e-235">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="f383e-236">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="f383e-236">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="f383e-237">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="f383e-237">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="f383e-238">ASP.NET Core 模組 stdout 記錄檔</span><span class="sxs-lookup"><span data-stu-id="f383e-238">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="f383e-239">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="f383e-239">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="f383e-240">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="f383e-240">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="f383e-241">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="f383e-241">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="f383e-242">在 [選取問題類別]  底下，選取 [Web 應用程式停止運作]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-242">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="f383e-243">在 [建議的解決方案]   > [啟用 Stdout 記錄檔重新導向]  底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-243">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="f383e-244">在 Kudu [診斷主控台]  中，將資料夾開啟至路徑 [site]   > [wwwroot]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-244">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="f383e-245">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f383e-245">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="f383e-246">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="f383e-246">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="f383e-247">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="f383e-247">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="f383e-248">選取 [儲存]  以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f383e-248">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="f383e-249">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="f383e-249">Make a request to the app.</span></span>
1. <span data-ttu-id="f383e-250">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="f383e-250">Return to the Azure portal.</span></span> <span data-ttu-id="f383e-251">選取 [開發工具]  區域中的 [進階工具]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="f383e-251">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="f383e-252">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-252">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f383e-253">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="f383e-253">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="f383e-254">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-254">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="f383e-255">選取 [LogFiles]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="f383e-255">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="f383e-256">檢查 [修改時間]  資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f383e-256">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="f383e-257">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="f383e-257">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="f383e-258">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="f383e-258">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="f383e-259">在 Kudu [診斷主控台]  中，返回路徑 [site]   > [wwwroot]  以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f383e-259">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="f383e-260">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="f383e-260">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="f383e-261">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="f383e-261">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="f383e-262">選取 [儲存]  以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="f383e-262">Select **Save** to save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="f383e-263">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="f383e-263">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="f383e-264">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="f383e-264">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="f383e-265">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="f383e-265">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="f383e-266">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="f383e-266">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f383e-267">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="f383e-267">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log"></a><span data-ttu-id="f383e-268">ASP.NET Core 模組偵錯記錄</span><span class="sxs-lookup"><span data-stu-id="f383e-268">ASP.NET Core Module debug log</span></span>

<span data-ttu-id="f383e-269">ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。</span><span class="sxs-lookup"><span data-stu-id="f383e-269">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="f383e-270">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="f383e-270">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="f383e-271">若要啟用增強型診斷記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="f383e-271">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="f383e-272">遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="f383e-272">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="f383e-273">重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="f383e-273">Redeploy the app.</span></span>
   * <span data-ttu-id="f383e-274">使用 Kudu 主控台，將[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所顯示的 `<handlerSettings>` 新增至即時應用程式的 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f383e-274">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="f383e-275">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-275">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="f383e-276">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-276">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f383e-277">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="f383e-277">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="f383e-278">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-278">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="f383e-279">將資料夾開啟至路徑 [site]   > [wwwroot]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-279">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="f383e-280">選取鉛筆圖示來編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f383e-280">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="f383e-281">新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。</span><span class="sxs-lookup"><span data-stu-id="f383e-281">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="f383e-282">選取 [儲存]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-282">Select the **Save** button.</span></span>
1. <span data-ttu-id="f383e-283">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-283">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="f383e-284">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-284">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f383e-285">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="f383e-285">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="f383e-286">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-286">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="f383e-287">將資料夾開啟至路徑 [site]   > [wwwroot]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-287">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="f383e-288">如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。</span><span class="sxs-lookup"><span data-stu-id="f383e-288">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="f383e-289">如果已提供路徑，請巡覽至記錄檔的位置。</span><span class="sxs-lookup"><span data-stu-id="f383e-289">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="f383e-290">使用檔案名稱旁的鉛筆圖示來開啟記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f383e-290">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="f383e-291">完成疑難排解時，請停用偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="f383e-291">Disable debug logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="f383e-292">若要停用增強型偵錯記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="f383e-292">To disable the enhanced debug log, perform either of the following:</span></span>
   * <span data-ttu-id="f383e-293">從 *web.config* 檔案本機移除 `<handlerSettings>` 並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="f383e-293">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
   * <span data-ttu-id="f383e-294">使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。</span><span class="sxs-lookup"><span data-stu-id="f383e-294">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="f383e-295">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="f383e-295">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="f383e-296">無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="f383e-296">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="f383e-297">記錄檔大小沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="f383e-297">There's no limit on log file size.</span></span> <span data-ttu-id="f383e-298">請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。</span><span class="sxs-lookup"><span data-stu-id="f383e-298">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="f383e-299">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="f383e-299">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f383e-300">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="f383e-300">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

## <a name="common-startup-errors"></a><span data-ttu-id="f383e-301">常見的啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="f383e-301">Common startup errors</span></span>

<span data-ttu-id="f383e-302">請參閱 <xref:host-and-deploy/azure-iis-errors-reference>。</span><span class="sxs-lookup"><span data-stu-id="f383e-302">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="f383e-303">參考主題涵蓋了大多數導致應用程式無法啟動的常見問題。</span><span class="sxs-lookup"><span data-stu-id="f383e-303">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="f383e-304">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="f383e-304">Slow or hanging app</span></span>

<span data-ttu-id="f383e-305">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="f383e-305">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="f383e-306">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="f383e-306">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="f383e-307">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="f383e-307">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="f383e-308">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="f383e-308">Remote debugging</span></span>

<span data-ttu-id="f383e-309">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="f383e-309">See the following topics:</span></span>

* <span data-ttu-id="f383e-310">[＜使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式＞的＜遠端偵錯 Web 應用程式＞一節](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure 文件)</span><span class="sxs-lookup"><span data-stu-id="f383e-310">[Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure documentation)</span></span>
* <span data-ttu-id="f383e-311">[在 Visual Studio 2017 中針對 Azure 中 IIS 上的 ASP.NET Core 進行遠端偵錯](/visualstudio/debugger/remote-debugging-azure) \(機器翻譯\) (Visual Studio 文件)</span><span class="sxs-lookup"><span data-stu-id="f383e-311">[Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (Visual Studio documentation)</span></span>

## <a name="application-insights"></a><span data-ttu-id="f383e-312">Application Insights</span><span class="sxs-lookup"><span data-stu-id="f383e-312">Application Insights</span></span>

<span data-ttu-id="f383e-313">[Application Insights](https://azure.microsoft.com/services/application-insights/) 提供來自 Azure App Service 中所裝載應用程式的遙測資料，包括錯誤記錄和報告功能。</span><span class="sxs-lookup"><span data-stu-id="f383e-313">[Application Insights](https://azure.microsoft.com/services/application-insights/) provides telemetry from apps hosted in the Azure App Service, including error logging and reporting features.</span></span> <span data-ttu-id="f383e-314">Application Insights 只能針對在應用程式啟動之後，應用程式記錄功能變成可用時所發生的錯誤提出報告。</span><span class="sxs-lookup"><span data-stu-id="f383e-314">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="f383e-315">如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="f383e-315">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="monitoring-blades"></a><span data-ttu-id="f383e-316">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="f383e-316">Monitoring blades</span></span>

<span data-ttu-id="f383e-317">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="f383e-317">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="f383e-318">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="f383e-318">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="f383e-319">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="f383e-319">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="f383e-320">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="f383e-320">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="f383e-321">在 [開發工具]  刀鋒視窗區段中，選取 [延伸模組]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="f383e-321">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="f383e-322">[ASP.NET Core 延伸模組]  應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="f383e-322">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="f383e-323">如果未安裝這些延伸模組，請選取 [新增]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-323">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="f383e-324">從清單中選擇 [ASP.NET Core 延伸模組]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-324">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="f383e-325">選取 [確定]  以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="f383e-325">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="f383e-326">在 [新增延伸模組]  刀鋒視窗上，選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-326">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="f383e-327">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="f383e-327">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="f383e-328">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="f383e-328">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="f383e-329">在 Azure 入口網站中，選取 [開發工具]  區域中的 [進階工具]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="f383e-329">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="f383e-330">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-330">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="f383e-331">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="f383e-331">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="f383e-332">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-332">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="f383e-333">將資料夾開啟至路徑 [site]   > [wwwroot]  ，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f383e-333">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="f383e-334">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="f383e-334">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="f383e-335">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="f383e-335">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="f383e-336">選取 [儲存]  以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f383e-336">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="f383e-337">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="f383e-337">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="f383e-338">在 Azure 入口網站中，選取 [診斷記錄檔]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="f383e-338">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="f383e-339">選取 [應用程式記錄 (檔案系統)]  和 [詳細錯誤訊息]  .的 [開啟]  開關。</span><span class="sxs-lookup"><span data-stu-id="f383e-339">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="f383e-340">選取刀鋒視窗頂端的 [儲存]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="f383e-340">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="f383e-341">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤]  的 [開啟]  開關。</span><span class="sxs-lookup"><span data-stu-id="f383e-341">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="f383e-342">選取 [記錄資料流]  刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔]  刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="f383e-342">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="f383e-343">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="f383e-343">Make a request to the app.</span></span>
1. <span data-ttu-id="f383e-344">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="f383e-344">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="f383e-345">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="f383e-345">Be sure to disable stdout logging when troubleshooting is complete.</span></span> <span data-ttu-id="f383e-346">請參閱 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)一節中的指示。</span><span class="sxs-lookup"><span data-stu-id="f383e-346">See the instructions in the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) section.</span></span>

<span data-ttu-id="f383e-347">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="f383e-347">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="f383e-348">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="f383e-348">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="f383e-349">從資訊看板的 [支援工具]  區域中，選取 [失敗要求追蹤記錄檔]  。</span><span class="sxs-lookup"><span data-stu-id="f383e-349">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="f383e-350">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和 [Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="f383e-350">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="f383e-351">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="f383e-351">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="f383e-352">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="f383e-352">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="f383e-353">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="f383e-353">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="f383e-354">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="f383e-354">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f383e-355">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="f383e-355">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f383e-356">其他資源</span><span class="sxs-lookup"><span data-stu-id="f383e-356">Additional resources</span></span>

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="f383e-357">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f383e-357">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="f383e-358">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="f383e-358">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="f383e-359">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="f383e-359">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="f383e-360">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="f383e-360">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="f383e-361">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="f383e-361">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="f383e-362">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="f383e-362">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
