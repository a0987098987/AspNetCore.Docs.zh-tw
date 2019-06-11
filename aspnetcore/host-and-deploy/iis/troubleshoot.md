---
title: 針對 IIS 上的 ASP.NET Core 進行疑難排解
author: guardrex
description: 了解如何診斷 ASP.NET Core 應用程式的 Internet Information Services (IIS) 部署問題。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: e4c93459f2030c7c0a55ea90e0cc8c8d30b76c51
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470455"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="fdb6c-103">針對 IIS 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="fdb6c-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="fdb6c-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fdb6c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fdb6c-105">本文說明以 [Internet Information Services (IIS)](/iis) 裝載 ASP.NET Core 應用程式時，如何診斷此應用程式的啟動問題。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="fdb6c-106">本文中資訊適用的裝載環境是 Windows Server 和 Windows 桌面上的 IIS。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fdb6c-107">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="fdb6c-108">在本機偵錯時發生的「502.5 - 處理序失敗」  或「500.30 - 啟動失敗」  可以使用本主題中的建議在該位置進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fdb6c-109">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="fdb6c-110">在本機偵錯時發生的「502.5 處理序失敗」  可以使用本主題中的建議來進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="fdb6c-111">其他疑難排解主題：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-111">Additional troubleshooting topics:</span></span>

<span data-ttu-id="fdb6c-112"><xref:host-and-deploy/azure-apps/troubleshoot> 雖然 App Service 使用 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)和 IIS 來裝載應用程式，但如需 App Service 特定的指示，請參閱其專屬的主題。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-112"><xref:host-and-deploy/azure-apps/troubleshoot> Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<span data-ttu-id="fdb6c-113"><xref:fundamentals/error-handling> 了解在本機系統上進行開發時，如何處理 ASP.NET Core 應用程式中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-113"><xref:fundamentals/error-handling> Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

<span data-ttu-id="fdb6c-114">[了解如何使用 Visual Studio 進行偵錯](/visualstudio/debugger/getting-started-with-the-debugger) 本主題介紹 Visual Studio 偵錯工具的功能。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-114">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="fdb6c-115">[使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\) 了解 Visual Studio Code 中內建的偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-115">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="fdb6c-116">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="fdb6c-116">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="fdb6c-117">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="fdb6c-117">502.5 Process Failure</span></span>

<span data-ttu-id="fdb6c-118">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-118">The worker process fails.</span></span> <span data-ttu-id="fdb6c-119">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-119">The app doesn't start.</span></span>

<span data-ttu-id="fdb6c-120">ASP.NET Core 模組嘗試啟動後端 dotnet 處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-120">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="fdb6c-121">通常從[應用程式事件記錄檔](#application-event-log)和 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="fdb6c-122">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-122">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="fdb6c-123">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-123">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="fdb6c-124">「共用的架構」  是一組安裝於電腦上且由 `Microsoft.AspNetCore.App` 之類的中繼套件所參考的組件 ( *.dll* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-124">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="fdb6c-125">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-125">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="fdb6c-126">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-126">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="fdb6c-127">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗]  錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-127">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![顯示 [502.5 處理序失敗] 頁面的瀏覽器視窗](troubleshoot/_static/process-failure-page.png)

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="fdb6c-129">500.30 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="fdb6c-129">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="fdb6c-130">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-130">The worker process fails.</span></span> <span data-ttu-id="fdb6c-131">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-131">The app doesn't start.</span></span>

<span data-ttu-id="fdb6c-132">ASP.NET Core 模組嘗試啟動 .NET Core CLR 同處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-132">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="fdb6c-133">通常從[應用程式事件記錄檔](#application-event-log)和 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-133">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="fdb6c-134">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-134">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="fdb6c-135">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-135">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="fdb6c-136">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="fdb6c-136">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="fdb6c-137">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-137">The worker process fails.</span></span> <span data-ttu-id="fdb6c-138">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-138">The app doesn't start.</span></span>

<span data-ttu-id="fdb6c-139">The ASP.NET Core 模組找不到 .NET Core CLR，並尋找同處理序要求處理常式 (*aspnetcorev2_inprocess.dll*)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-139">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="fdb6c-140">請檢查︰</span><span class="sxs-lookup"><span data-stu-id="fdb6c-140">Check that:</span></span>

* <span data-ttu-id="fdb6c-141">應用程式以 [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet 套件或 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)為目標。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-141">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="fdb6c-142">應用程式設為目標的 ASP.NET Core 共用架構版本有安裝在目標機器上。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-142">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="fdb6c-143">500.0 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="fdb6c-143">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="fdb6c-144">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-144">The worker process fails.</span></span> <span data-ttu-id="fdb6c-145">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-145">The app doesn't start.</span></span>

<span data-ttu-id="fdb6c-146">ASP.NET Core 模組找不到跨處理序裝載要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-146">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="fdb6c-147">請確定 *aspnetcorev2_outofprocess.dll* 出現在子資料夾中，且位於 *aspnetcorev2.dll* 旁。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-147">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="fdb6c-148">500.31 ANCM 找不到原生相依性</span><span class="sxs-lookup"><span data-stu-id="fdb6c-148">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="fdb6c-149">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-149">The worker process fails.</span></span> <span data-ttu-id="fdb6c-150">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-150">The app doesn't start.</span></span>

<span data-ttu-id="fdb6c-151">ASP.NET Core 模組嘗試啟動 .NET Core 執行階段同處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-151">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="fdb6c-152">此啟動失敗的最常見原因是當 `Microsoft.NETCore.App` 或 `Microsoft.AspNetCore.App` 執行階段未安裝時。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-152">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="fdb6c-153">如果應用程式部署至目標 ASP.NET Core 3.0，但電腦上無該版本，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-153">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="fdb6c-154">範例錯誤訊息如下：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-154">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="fdb6c-155">錯誤訊息會列出所有已安裝 .NET Core 版本和應用程式所要求的版本。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-155">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="fdb6c-156">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-156">To fix this error, either:</span></span>

* <span data-ttu-id="fdb6c-157">在電腦上安裝適當的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-157">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="fdb6c-158">將應用程式的目標 .NET Core 版本變更為電腦上版本。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-158">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="fdb6c-159">將應用程式發佈為[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-159">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="fdb6c-160">在開發過程中執行時 (`ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`)，特定的錯誤會寫入至 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-160">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="fdb6c-161">處理序啟動失敗的原因也會列在[應用程式事件記錄檔](#application-event-log)中。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-161">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="fdb6c-162">500.32 ANCM 無法載入 dll</span><span class="sxs-lookup"><span data-stu-id="fdb6c-162">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="fdb6c-163">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-163">The worker process fails.</span></span> <span data-ttu-id="fdb6c-164">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-164">The app doesn't start.</span></span>

<span data-ttu-id="fdb6c-165">此錯誤最常見原因是針對不相容的處理器架構發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-165">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="fdb6c-166">如果背景工作處理序執行為 32 位元應用程式，而此應用程式已發佈至目標 64 位元，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-166">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="fdb6c-167">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-167">To fix this error, either:</span></span>

* <span data-ttu-id="fdb6c-168">針對相同的處理器架構，將應用程式重新發佈為背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-168">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="fdb6c-169">將應用程式發佈為[架構相依部署](/dotnet/core/deploying/#framework-dependent-executables-fde)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-169">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="fdb6c-170">500.33 ANCM 要求處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="fdb6c-170">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="fdb6c-171">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-171">The worker process fails.</span></span> <span data-ttu-id="fdb6c-172">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-172">The app doesn't start.</span></span>

<span data-ttu-id="fdb6c-173">應用程式未參考 `Microsoft.AspNetCore.App` 架構。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-173">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="fdb6c-174">ASP.NET Core 模組只裝載以 `Microsoft.AspNetCore.App` 架構為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-174">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the ASP.NET Core Module.</span></span>

<span data-ttu-id="fdb6c-175">若要修正這個錯誤，請確認應用程式以 `Microsoft.AspNetCore.App` 架構為目標。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-175">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="fdb6c-176">檢查 `.runtimeconfig.json` 以驗證應用程式是否以該架構為目標。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-176">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="fdb6c-177">500.34 ANCM 不支援混合式裝載模型</span><span class="sxs-lookup"><span data-stu-id="fdb6c-177">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="fdb6c-178">背景工作處理序無法在相同的程序中執行同處理序應用程式和跨處理序應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-178">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="fdb6c-179">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-179">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="fdb6c-180">500.35 ANCM 同一程序中有多個同處理序應用程式</span><span class="sxs-lookup"><span data-stu-id="fdb6c-180">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="fdb6c-181">背景工作處理序無法在相同的程序中執行同處理序應用程式和跨處理序應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-181">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="fdb6c-182">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-182">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="fdb6c-183">500.36 ANCM 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="fdb6c-183">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="fdb6c-184">跨處理序要求處理常式 *aspnetcorev2_outofprocess.dll* 不在 *aspnetcorev2.dll* 檔案旁邊。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-184">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="fdb6c-185">這表示 ASP.NET Core 模組安裝損毀。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-185">This indicates a corrupted installation of the ASP.NET Core Module.</span></span>

<span data-ttu-id="fdb6c-186">若要修正這個錯誤，請修復 [.NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (適用於 IIS) 或 Visual Studio (適用於 IIS Express) 安裝。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-186">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="fdb6c-187">500.37 ANCM 無法在啟動時間限制內啟動</span><span class="sxs-lookup"><span data-stu-id="fdb6c-187">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="fdb6c-188">ANCM 無法在提供的啟動時間限制內啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-188">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="fdb6c-189">根據預設，逾時值為 120 秒。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-189">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="fdb6c-190">在同一部電腦上啟動大量的應用程式時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-190">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="fdb6c-191">檢查伺服器在啟動期間是否出現 CPU/記憶體的使用量尖峰。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-191">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="fdb6c-192">多個應用程式的啟動程序可能需要交錯進行。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-192">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="fdb6c-193">500.30 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="fdb6c-193">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="fdb6c-194">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-194">The worker process fails.</span></span> <span data-ttu-id="fdb6c-195">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-195">The app doesn't start.</span></span>

<span data-ttu-id="fdb6c-196">ASP.NET Core 模組嘗試啟動 .NET Core 執行階段同處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-196">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="fdb6c-197">通常從[應用程式事件記錄檔](#application-event-log)和 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-197">The cause of a process startup failure is usually determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="fdb6c-198">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="fdb6c-198">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="fdb6c-199">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-199">The worker process fails.</span></span> <span data-ttu-id="fdb6c-200">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-200">The app doesn't start.</span></span>

<span data-ttu-id="fdb6c-201">載入 ASP.NET Core 模組元件時發生未知的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-201">An unknown error occurred loading ASP.NET Core Module components.</span></span> <span data-ttu-id="fdb6c-202">採取下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-202">Take one of the following actions:</span></span>

* <span data-ttu-id="fdb6c-203">連絡 [Microsoft 支援服務](https://support.microsoft.com/oas/default.aspx?prid=15832) (依序選取 [開發人員工具]  和 [ASP.NET Core]  )。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-203">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="fdb6c-204">在 Stack Overflow 上詢問問題。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-204">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="fdb6c-205">在我們的 [GitHub 存放庫](https://github.com/aspnet/AspNetCore)提出問題。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-205">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="fdb6c-206">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="fdb6c-206">500 Internal Server Error</span></span>

<span data-ttu-id="fdb6c-207">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-207">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="fdb6c-208">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-208">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="fdb6c-209">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」  的形式出現。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-209">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="fdb6c-210">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-210">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="fdb6c-211">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-211">From the server's perspective, that's correct.</span></span> <span data-ttu-id="fdb6c-212">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-212">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="fdb6c-213">請在伺服器上[於命令提示字元中執行應用程式](#run-the-app-at-a-command-prompt)或[啟用 ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-213">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="fdb6c-214">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="fdb6c-214">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="fdb6c-215">應用程式無法啟動，因為無法載入應用程式的組件 ( *.dll*)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-215">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="fdb6c-216">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-216">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="fdb6c-217">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-217">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="fdb6c-218">在 IIS 管理員的 [應用程式集區]  中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-218">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="fdb6c-219">在 [動作]  面板中，選取 [編輯應用程式集區]  下的 [進階設定]  。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-219">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="fdb6c-220">設定 [啟用 32 位元應用程式]  ：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-220">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="fdb6c-221">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-221">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="fdb6c-222">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-222">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="fdb6c-223">連線重設</span><span class="sxs-lookup"><span data-stu-id="fdb6c-223">Connection reset</span></span>

<span data-ttu-id="fdb6c-224">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」  。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-224">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="fdb6c-225">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-225">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="fdb6c-226">這類錯誤會在用戶端上顯示為「連線重設」  錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-226">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="fdb6c-227">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-227">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="fdb6c-228">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="fdb6c-228">Default startup limits</span></span>

<span data-ttu-id="fdb6c-229">ASP.NET Core 模組上已設定預設的 *startupTimeLimit* 120 秒。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-229">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="fdb6c-230">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-230">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="fdb6c-231">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-231">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="fdb6c-232">針對應用程式啟動錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="fdb6c-232">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="fdb6c-233">啟用 ASP.NET Core 模組偵錯記錄</span><span class="sxs-lookup"><span data-stu-id="fdb6c-233">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="fdb6c-234">將下列處理常式設定新增至應用程式的 *web.config* 檔案，以啟用 ASP.NET Core 模組偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-234">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="fdb6c-235">確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-235">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="fdb6c-236">應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="fdb6c-236">Application Event Log</span></span>

<span data-ttu-id="fdb6c-237">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-237">Access the Application Event Log:</span></span>

1. <span data-ttu-id="fdb6c-238">開啟 [開始] 功能表，搜尋**事件檢視器**，然後選取 [事件檢視器]  應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-238">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="fdb6c-239">在 [事件檢視器]  中，開啟 [Windows 記錄]  節點。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-239">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="fdb6c-240">選取 [應用程式]  以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-240">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="fdb6c-241">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-241">Search for errors associated with the failing app.</span></span> <span data-ttu-id="fdb6c-242">錯誤在 [來源]  資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-242">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="fdb6c-243">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="fdb6c-243">Run the app at a command prompt</span></span>

<span data-ttu-id="fdb6c-244">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-244">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="fdb6c-245">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-245">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="fdb6c-246">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="fdb6c-246">Framework-dependent deployment</span></span>

<span data-ttu-id="fdb6c-247">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-247">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="fdb6c-248">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-248">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="fdb6c-249">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-249">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="fdb6c-250">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-250">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="fdb6c-251">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-251">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="fdb6c-252">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-252">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="fdb6c-253">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-253">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="fdb6c-254">自封式部署</span><span class="sxs-lookup"><span data-stu-id="fdb6c-254">Self-contained deployment</span></span>

<span data-ttu-id="fdb6c-255">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-255">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="fdb6c-256">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-256">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="fdb6c-257">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-257">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="fdb6c-258">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-258">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="fdb6c-259">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-259">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="fdb6c-260">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-260">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="fdb6c-261">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-261">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="fdb6c-262">ASP.NET Core 模組 stdout 記錄檔</span><span class="sxs-lookup"><span data-stu-id="fdb6c-262">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="fdb6c-263">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-263">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="fdb6c-264">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-264">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="fdb6c-265">如果 [logs]  資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-265">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="fdb6c-266">如需有關如何讓 MSBuild 在部署中自動建立 [logs]  資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-266">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="fdb6c-267">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-267">Edit the *web.config* file.</span></span> <span data-ttu-id="fdb6c-268">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs]  資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-268">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="fdb6c-269">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-269">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="fdb6c-270">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-270">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="fdb6c-271">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-271">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="fdb6c-272">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-272">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="fdb6c-273">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-273">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="fdb6c-274">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-274">Make a request to the app.</span></span>
1. <span data-ttu-id="fdb6c-275">瀏覽至 [logs]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-275">Navigate to the *logs* folder.</span></span> <span data-ttu-id="fdb6c-276">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-276">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="fdb6c-277">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-277">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fdb6c-278">完成疑難排解時，請停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-278">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="fdb6c-279">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-279">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="fdb6c-280">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-280">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="fdb6c-281">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-281">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="fdb6c-282">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-282">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="fdb6c-283">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-283">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="fdb6c-284">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-284">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="fdb6c-285">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-285">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="fdb6c-286">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="fdb6c-286">Enable the Developer Exception Page</span></span>

<span data-ttu-id="fdb6c-287">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-287">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="fdb6c-288">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-288">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="fdb6c-289">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-289">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="fdb6c-290">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-290">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="fdb6c-291">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-291">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="fdb6c-292">常見的啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="fdb6c-292">Common startup errors</span></span>

<span data-ttu-id="fdb6c-293">請參閱 <xref:host-and-deploy/azure-iis-errors-reference>。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-293">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="fdb6c-294">參考主題涵蓋了大多數導致應用程式無法啟動的常見問題。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-294">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="fdb6c-295">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="fdb6c-295">Obtain data from an app</span></span>

<span data-ttu-id="fdb6c-296">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-296">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="fdb6c-297">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-297">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="fdb6c-298">建立傾印</span><span class="sxs-lookup"><span data-stu-id="fdb6c-298">Create a dump</span></span>

<span data-ttu-id="fdb6c-299">「傾印」  是系統記憶體的快照集，可協助您判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-299">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="fdb6c-300">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="fdb6c-300">App crashes or encounters an exception</span></span>

<span data-ttu-id="fdb6c-301">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-301">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="fdb6c-302">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-302">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="fdb6c-303">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-303">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="fdb6c-304">執行 [EnableDumps PowerShell 指令碼](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-304">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="fdb6c-305">如果應用程式是使用[同處理序主控模型](xref:fundamentals/servers/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-305">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="fdb6c-306">如果應用程式是使用[跨處理序主控模型](xref:fundamentals/servers/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-306">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="fdb6c-307">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-307">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="fdb6c-308">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-308">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="fdb6c-309">如果應用程式是使用[同處理序主控模型](xref:fundamentals/servers/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-309">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="fdb6c-310">如果應用程式是使用[跨處理序主控模型](xref:fundamentals/servers/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-310">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="fdb6c-311">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-311">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="fdb6c-312">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-312">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="fdb6c-313">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-313">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="fdb6c-314">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="fdb6c-314">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="fdb6c-315">當應用程式「停止回應」  (停止回應但未損毀)、在啟動期間失敗，或正常執行時，請參閱[使用者模式傾印檔案：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)，選取適當的工具來產生傾印。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-315">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="fdb6c-316">分析傾印</span><span class="sxs-lookup"><span data-stu-id="fdb6c-316">Analyze the dump</span></span>

<span data-ttu-id="fdb6c-317">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-317">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="fdb6c-318">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-318">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="fdb6c-319">遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="fdb6c-319">Remote debugging</span></span>

<span data-ttu-id="fdb6c-320">請參閱 Visual Studio 文件中的[在 Visual Studio 2017 中針對遠端 IIS 電腦上的 ASP.NET Core 進行遠端偵錯](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) \(機器翻譯\)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-320">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="fdb6c-321">Application Insights</span><span class="sxs-lookup"><span data-stu-id="fdb6c-321">Application Insights</span></span>

<span data-ttu-id="fdb6c-322">[Application Insights](/azure/application-insights/) 提供來自 IIS 所裝載應用程式的遙測資料，包括錯誤記錄和報告功能。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-322">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="fdb6c-323">Application Insights 只能針對在應用程式啟動之後，應用程式記錄功能變成可用時所發生的錯誤提出報告。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-323">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="fdb6c-324">如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-324">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="fdb6c-325">額外建議</span><span class="sxs-lookup"><span data-stu-id="fdb6c-325">Additional advice</span></span>

<span data-ttu-id="fdb6c-326">有時，在升級開發電腦上的 .NET Core SDK 或應用程式內的套件版本之後，正常運作的應用程式便立即發生失敗。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-326">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="fdb6c-327">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-327">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="fdb6c-328">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="fdb6c-328">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="fdb6c-329">刪除 [bin]  和 [obj]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-329">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="fdb6c-330">清除位於 *%UserProfile%\\.nuget\\packages* 和 *%LocalAppData%\\Nuget\\v3-cache*的套件快取。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-330">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="fdb6c-331">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-331">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="fdb6c-332">先確認已將伺服器上先前的部署完全刪除，再重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-332">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="fdb6c-333">有一個便利的方式可以清除套件快取，就是從命令提示字元執行 `dotnet nuget locals all --clear`。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-333">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="fdb6c-334">您也可以使用 [nuget.exe](https://www.nuget.org/downloads) 工具並執行 `nuget locals all -clear` 命令，來清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-334">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="fdb6c-335">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="fdb6c-335">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fdb6c-336">其他資源</span><span class="sxs-lookup"><span data-stu-id="fdb6c-336">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
