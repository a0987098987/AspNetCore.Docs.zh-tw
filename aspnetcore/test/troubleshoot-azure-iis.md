---
title: 疑難排解 Azure App Service 和 IIS 上的 ASP.NET Core
author: guardrex
description: 瞭解如何診斷 ASP.NET Core 應用程式的 Azure App Service 和 Internet Information Services (IIS) 部署問題。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2019
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 46d4a11c594844e059fa8555255d7321f7b48631
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308791"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="716a2-103">疑難排解 Azure App Service 和 IIS 上的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="716a2-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="716a2-104">By [Luke Latham](https://github.com/guardrex)和[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="716a2-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="716a2-105">本文提供有關一般應用程式啟動錯誤的資訊, 以及如何在將應用程式部署至 Azure App Service 或 IIS 時診斷錯誤的指示:</span><span class="sxs-lookup"><span data-stu-id="716a2-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="716a2-106">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="716a2-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="716a2-107">說明常見的啟動 HTTP 狀態碼案例。</span><span class="sxs-lookup"><span data-stu-id="716a2-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="716a2-108">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="716a2-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="716a2-109">提供部署至 Azure App Service 之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="716a2-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="716a2-110">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="716a2-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="716a2-111">提供部署至 IIS 或在本機 IIS Express 上執行之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="716a2-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="716a2-112">本指南適用于 Windows Server 和 Windows 桌面部署。</span><span class="sxs-lookup"><span data-stu-id="716a2-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="716a2-113">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="716a2-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="716a2-114">說明當一致封裝在執行主要升級或變更封裝版本時, 中斷應用程式時該怎麼辦。</span><span class="sxs-lookup"><span data-stu-id="716a2-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="716a2-115">其他資源</span><span class="sxs-lookup"><span data-stu-id="716a2-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="716a2-116">列出其他疑難排解主題。</span><span class="sxs-lookup"><span data-stu-id="716a2-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="716a2-117">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="716a2-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="716a2-118">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="716a2-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="716a2-119">使用本主題中的建議, 可以診斷在本機進行偵錯工具時所發生的*502.5-進程失敗*或*500.30 啟動失敗*。</span><span class="sxs-lookup"><span data-stu-id="716a2-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="716a2-120">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="716a2-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="716a2-121">使用本主題中的建議, 可以診斷本機上的偵錯工具發生的*502.5 進程失敗*。</span><span class="sxs-lookup"><span data-stu-id="716a2-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="716a2-122">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="716a2-122">500 Internal Server Error</span></span>

<span data-ttu-id="716a2-123">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="716a2-123">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="716a2-124">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="716a2-124">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="716a2-125">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」  的形式出現。</span><span class="sxs-lookup"><span data-stu-id="716a2-125">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="716a2-126">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-126">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="716a2-127">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="716a2-127">From the server's perspective, that's correct.</span></span> <span data-ttu-id="716a2-128">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="716a2-128">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="716a2-129">在伺服器上的命令提示字元中執行應用程式, 或啟用 ASP.NET Core 模組 stdout 記錄檔, 以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="716a2-129">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="716a2-130">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="716a2-130">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="716a2-131">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-131">The worker process fails.</span></span> <span data-ttu-id="716a2-132">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-132">The app doesn't start.</span></span>

<span data-ttu-id="716a2-133">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)找不到 .NET Core CLR, 而且找不到同進程要求處理常式 (*aspnetcorev2_inprocess .dll*)。</span><span class="sxs-lookup"><span data-stu-id="716a2-133">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="716a2-134">請檢查︰</span><span class="sxs-lookup"><span data-stu-id="716a2-134">Check that:</span></span>

* <span data-ttu-id="716a2-135">應用程式以 [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet 套件或 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)為目標。</span><span class="sxs-lookup"><span data-stu-id="716a2-135">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="716a2-136">應用程式設為目標的 ASP.NET Core 共用架構版本有安裝在目標機器上。</span><span class="sxs-lookup"><span data-stu-id="716a2-136">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="716a2-137">500.0 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="716a2-137">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="716a2-138">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-138">The worker process fails.</span></span> <span data-ttu-id="716a2-139">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-139">The app doesn't start.</span></span>

<span data-ttu-id="716a2-140">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)找不到跨進程主控要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="716a2-140">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="716a2-141">請確定 *aspnetcorev2_outofprocess.dll* 出現在子資料夾中，且位於 *aspnetcorev2.dll* 旁。</span><span class="sxs-lookup"><span data-stu-id="716a2-141">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="716a2-142">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="716a2-142">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="716a2-143">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-143">The worker process fails.</span></span> <span data-ttu-id="716a2-144">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-144">The app doesn't start.</span></span>

<span data-ttu-id="716a2-145">載入[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)元件時發生未知的錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-145">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="716a2-146">採取下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="716a2-146">Take one of the following actions:</span></span>

* <span data-ttu-id="716a2-147">連絡 [Microsoft 支援服務](https://support.microsoft.com/oas/default.aspx?prid=15832) (依序選取 [開發人員工具]  和 [ASP.NET Core]  )。</span><span class="sxs-lookup"><span data-stu-id="716a2-147">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="716a2-148">在 Stack Overflow 上詢問問題。</span><span class="sxs-lookup"><span data-stu-id="716a2-148">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="716a2-149">在我們的 [GitHub 存放庫](https://github.com/aspnet/AspNetCore)提出問題。</span><span class="sxs-lookup"><span data-stu-id="716a2-149">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="716a2-150">500.30 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="716a2-150">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="716a2-151">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-151">The worker process fails.</span></span> <span data-ttu-id="716a2-152">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-152">The app doesn't start.</span></span>

<span data-ttu-id="716a2-153">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動 .NET Core CLR 同進程, 但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-153">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="716a2-154">通常會從應用程式事件記錄檔中的專案和 ASP.NET Core 模組 stdout 記錄檔中判斷進程啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="716a2-154">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="716a2-155">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="716a2-155">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="716a2-156">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="716a2-156">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="716a2-157">500.31 ANCM 找不到原生相依性</span><span class="sxs-lookup"><span data-stu-id="716a2-157">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="716a2-158">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-158">The worker process fails.</span></span> <span data-ttu-id="716a2-159">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-159">The app doesn't start.</span></span>

<span data-ttu-id="716a2-160">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試在同進程中啟動 .net Core 執行時間, 但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-160">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="716a2-161">此啟動失敗的最常見原因是當 `Microsoft.NETCore.App` 或 `Microsoft.AspNetCore.App` 執行階段未安裝時。</span><span class="sxs-lookup"><span data-stu-id="716a2-161">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="716a2-162">如果應用程式部署至目標 ASP.NET Core 3.0，但電腦上無該版本，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-162">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="716a2-163">範例錯誤訊息如下：</span><span class="sxs-lookup"><span data-stu-id="716a2-163">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="716a2-164">錯誤訊息會列出所有已安裝 .NET Core 版本和應用程式所要求的版本。</span><span class="sxs-lookup"><span data-stu-id="716a2-164">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="716a2-165">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="716a2-165">To fix this error, either:</span></span>

* <span data-ttu-id="716a2-166">在電腦上安裝適當的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="716a2-166">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="716a2-167">將應用程式的目標 .NET Core 版本變更為電腦上版本。</span><span class="sxs-lookup"><span data-stu-id="716a2-167">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="716a2-168">將應用程式發佈為[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="716a2-168">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="716a2-169">在開發過程中執行時 (`ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`)，特定的錯誤會寫入至 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="716a2-169">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="716a2-170">在應用程式事件記錄檔中也可以找到進程啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="716a2-170">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="716a2-171">500.32 ANCM 無法載入 dll</span><span class="sxs-lookup"><span data-stu-id="716a2-171">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="716a2-172">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-172">The worker process fails.</span></span> <span data-ttu-id="716a2-173">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-173">The app doesn't start.</span></span>

<span data-ttu-id="716a2-174">此錯誤最常見原因是針對不相容的處理器架構發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-174">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="716a2-175">如果背景工作處理序執行為 32 位元應用程式，而此應用程式已發佈至目標 64 位元，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-175">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="716a2-176">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="716a2-176">To fix this error, either:</span></span>

* <span data-ttu-id="716a2-177">針對相同的處理器架構，將應用程式重新發佈為背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="716a2-177">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="716a2-178">將應用程式發佈為[架構相依部署](/dotnet/core/deploying/#framework-dependent-executables-fde)。</span><span class="sxs-lookup"><span data-stu-id="716a2-178">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="716a2-179">500.33 ANCM 要求處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="716a2-179">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="716a2-180">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-180">The worker process fails.</span></span> <span data-ttu-id="716a2-181">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-181">The app doesn't start.</span></span>

<span data-ttu-id="716a2-182">應用程式未參考 `Microsoft.AspNetCore.App` 架構。</span><span class="sxs-lookup"><span data-stu-id="716a2-182">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="716a2-183">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)只能主控`Microsoft.AspNetCore.App`以架構為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-183">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="716a2-184">若要修正這個錯誤，請確認應用程式以 `Microsoft.AspNetCore.App` 架構為目標。</span><span class="sxs-lookup"><span data-stu-id="716a2-184">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="716a2-185">檢查 `.runtimeconfig.json` 以驗證應用程式是否以該架構為目標。</span><span class="sxs-lookup"><span data-stu-id="716a2-185">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="716a2-186">500.34 ANCM 不支援混合式裝載模型</span><span class="sxs-lookup"><span data-stu-id="716a2-186">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="716a2-187">背景工作處理序無法在相同的程序中執行同處理序應用程式和跨處理序應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-187">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="716a2-188">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-188">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="716a2-189">500.35 ANCM 同一程序中有多個同處理序應用程式</span><span class="sxs-lookup"><span data-stu-id="716a2-189">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="716a2-190">背景工作處理序無法在相同的程序中執行同處理序應用程式和跨處理序應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-190">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="716a2-191">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-191">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="716a2-192">500.36 ANCM 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="716a2-192">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="716a2-193">跨處理序要求處理常式 *aspnetcorev2_outofprocess.dll* 不在 *aspnetcorev2.dll* 檔案旁邊。</span><span class="sxs-lookup"><span data-stu-id="716a2-193">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="716a2-194">這表示[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)安裝損毀。</span><span class="sxs-lookup"><span data-stu-id="716a2-194">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="716a2-195">若要修正這個錯誤，請修復 [.NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (適用於 IIS) 或 Visual Studio (適用於 IIS Express) 安裝。</span><span class="sxs-lookup"><span data-stu-id="716a2-195">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="716a2-196">500.37 ANCM 無法在啟動時間限制內啟動</span><span class="sxs-lookup"><span data-stu-id="716a2-196">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="716a2-197">ANCM 無法在提供的啟動時間限制內啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-197">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="716a2-198">根據預設，逾時值為 120 秒。</span><span class="sxs-lookup"><span data-stu-id="716a2-198">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="716a2-199">在同一部電腦上啟動大量的應用程式時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-199">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="716a2-200">檢查伺服器在啟動期間是否出現 CPU/記憶體的使用量尖峰。</span><span class="sxs-lookup"><span data-stu-id="716a2-200">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="716a2-201">多個應用程式的啟動程序可能需要交錯進行。</span><span class="sxs-lookup"><span data-stu-id="716a2-201">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="716a2-202">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="716a2-202">502.5 Process Failure</span></span>

<span data-ttu-id="716a2-203">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-203">The worker process fails.</span></span> <span data-ttu-id="716a2-204">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-204">The app doesn't start.</span></span>

<span data-ttu-id="716a2-205">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-205">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="716a2-206">通常會從應用程式事件記錄檔中的專案和 ASP.NET Core 模組 stdout 記錄檔中判斷進程啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="716a2-206">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="716a2-207">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="716a2-207">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="716a2-208">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="716a2-208">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="716a2-209">「共用的架構」  是一組安裝於電腦上且由 `Microsoft.AspNetCore.App` 之類的中繼套件所參考的組件 ( *.dll* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="716a2-209">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="716a2-210">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="716a2-210">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="716a2-211">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="716a2-211">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="716a2-212">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗]  錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="716a2-212">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="716a2-213">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="716a2-213">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="716a2-214">應用程式無法啟動，因為無法載入應用程式的組件 ( *.dll*)。</span><span class="sxs-lookup"><span data-stu-id="716a2-214">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="716a2-215">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-215">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="716a2-216">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="716a2-216">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="716a2-217">在 IIS 管理員的 [應用程式集區]  中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="716a2-217">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="716a2-218">在 [動作]  面板中，選取 [編輯應用程式集區]  下的 [進階設定]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-218">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="716a2-219">設定 [啟用 32 位元應用程式]  ：</span><span class="sxs-lookup"><span data-stu-id="716a2-219">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="716a2-220">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="716a2-220">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="716a2-221">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="716a2-221">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="716a2-222">連線重設</span><span class="sxs-lookup"><span data-stu-id="716a2-222">Connection reset</span></span>

<span data-ttu-id="716a2-223">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」  。</span><span class="sxs-lookup"><span data-stu-id="716a2-223">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="716a2-224">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-224">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="716a2-225">這類錯誤會在用戶端上顯示為「連線重設」  錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-225">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="716a2-226">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="716a2-226">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="716a2-227">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="716a2-227">Default startup limits</span></span>

<span data-ttu-id="716a2-228">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是以120秒的預設*startupTimeLimit*來設定。</span><span class="sxs-lookup"><span data-stu-id="716a2-228">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="716a2-229">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="716a2-229">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="716a2-230">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="716a2-230">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="716a2-231">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="716a2-231">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="716a2-232">應用程式事件記錄檔 (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="716a2-232">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="716a2-233">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題]  刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="716a2-233">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="716a2-234">在 Azure 入口網站的 [應用程式服務]  中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-234">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="716a2-235">選取 [診斷並解決問題]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-235">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="716a2-236">選取 [診斷工具]  標題。</span><span class="sxs-lookup"><span data-stu-id="716a2-236">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="716a2-237">在 [支援工具]  下，選取 [應用程式事件]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-237">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="716a2-238">檢查 [來源]  資料行中 *IIS AspNetCoreModule* 或 *IIS AspNetCoreModule V2* 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-238">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="716a2-239">除了使用 [診斷並解決問題]  刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="716a2-239">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="716a2-240">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-240">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="716a2-241">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-241">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="716a2-242">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="716a2-242">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="716a2-243">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-243">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="716a2-244">開啟 [LogFiles]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="716a2-244">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="716a2-245">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="716a2-245">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="716a2-246">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="716a2-246">Examine the log.</span></span> <span data-ttu-id="716a2-247">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="716a2-247">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="716a2-248">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="716a2-248">Run the app in the Kudu console</span></span>

<span data-ttu-id="716a2-249">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="716a2-249">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="716a2-250">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="716a2-250">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="716a2-251">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-251">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="716a2-252">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-252">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="716a2-253">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="716a2-253">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="716a2-254">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-254">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="716a2-255">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="716a2-255">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="716a2-256">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="716a2-256">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="716a2-257">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="716a2-257">Run the app:</span></span>
   * <span data-ttu-id="716a2-258">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="716a2-258">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="716a2-259">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="716a2-259">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="716a2-260">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="716a2-260">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="716a2-261">**在預覽版本上執行的 Framework 相依部署**</span><span class="sxs-lookup"><span data-stu-id="716a2-261">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="716a2-262">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="716a2-262">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="716a2-263">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="716a2-263">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="716a2-264">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="716a2-264">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="716a2-265">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="716a2-265">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="716a2-266">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="716a2-266">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="716a2-267">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="716a2-267">**Current release**</span></span>

* <span data-ttu-id="716a2-268">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="716a2-268">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="716a2-269">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="716a2-269">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="716a2-270">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="716a2-270">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="716a2-271">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="716a2-271">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="716a2-272">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="716a2-272">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="716a2-273">**在預覽版本上執行的 Framework 相依部署**</span><span class="sxs-lookup"><span data-stu-id="716a2-273">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="716a2-274">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="716a2-274">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="716a2-275">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="716a2-275">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="716a2-276">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="716a2-276">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="716a2-277">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="716a2-277">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="716a2-278">ASP.NET Core 模組 stdout 記錄檔 (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="716a2-278">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="716a2-279">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="716a2-279">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="716a2-280">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="716a2-280">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="716a2-281">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="716a2-281">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="716a2-282">在 [選取問題類別]  底下，選取 [Web 應用程式停止運作]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-282">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="716a2-283">在 [建議的解決方案]   > [啟用 Stdout 記錄檔重新導向]  底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-283">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="716a2-284">在 Kudu [診斷主控台]  中，將資料夾開啟至路徑 [site]   > [wwwroot]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-284">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="716a2-285">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-285">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="716a2-286">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="716a2-286">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="716a2-287">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="716a2-287">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="716a2-288">選取 [儲存]  以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-288">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="716a2-289">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="716a2-289">Make a request to the app.</span></span>
1. <span data-ttu-id="716a2-290">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="716a2-290">Return to the Azure portal.</span></span> <span data-ttu-id="716a2-291">選取 [開發工具]  區域中的 [進階工具]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="716a2-291">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="716a2-292">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-292">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="716a2-293">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="716a2-293">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="716a2-294">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-294">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="716a2-295">選取 [LogFiles]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="716a2-295">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="716a2-296">檢查 [修改時間]  資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="716a2-296">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="716a2-297">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-297">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="716a2-298">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="716a2-298">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="716a2-299">在 Kudu [診斷主控台]  中，返回路徑 [site]   > [wwwroot]  以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-299">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="716a2-300">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-300">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="716a2-301">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="716a2-301">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="716a2-302">選取 [儲存]  以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-302">Select **Save** to save the file.</span></span>

<span data-ttu-id="716a2-303">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="716a2-303">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="716a2-304">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-304">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="716a2-305">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="716a2-305">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="716a2-306">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="716a2-306">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="716a2-307">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="716a2-307">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="716a2-308">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="716a2-308">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="716a2-309">ASP.NET Core 模組的 debug 記錄檔 (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="716a2-309">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="716a2-310">ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。</span><span class="sxs-lookup"><span data-stu-id="716a2-310">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="716a2-311">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="716a2-311">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="716a2-312">若要啟用增強型診斷記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="716a2-312">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="716a2-313">遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="716a2-313">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="716a2-314">重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-314">Redeploy the app.</span></span>
   * <span data-ttu-id="716a2-315">使用 Kudu 主控台，將[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所顯示的 `<handlerSettings>` 新增至即時應用程式的 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="716a2-315">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="716a2-316">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-316">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="716a2-317">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-317">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="716a2-318">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="716a2-318">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="716a2-319">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-319">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="716a2-320">將資料夾開啟至路徑 [site]   > [wwwroot]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-320">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="716a2-321">選取鉛筆圖示來編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-321">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="716a2-322">新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。</span><span class="sxs-lookup"><span data-stu-id="716a2-322">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="716a2-323">選取 [儲存]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-323">Select the **Save** button.</span></span>
1. <span data-ttu-id="716a2-324">在 [開發工具]  區域中，開啟 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-324">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="716a2-325">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-325">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="716a2-326">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="716a2-326">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="716a2-327">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-327">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="716a2-328">將資料夾開啟至路徑 [site]   > [wwwroot]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-328">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="716a2-329">如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。</span><span class="sxs-lookup"><span data-stu-id="716a2-329">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="716a2-330">如果已提供路徑，請巡覽至記錄檔的位置。</span><span class="sxs-lookup"><span data-stu-id="716a2-330">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="716a2-331">使用檔案名稱旁的鉛筆圖示來開啟記錄檔。</span><span class="sxs-lookup"><span data-stu-id="716a2-331">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="716a2-332">完成疑難排解時，請停用偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="716a2-332">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="716a2-333">若要停用增強型偵錯記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="716a2-333">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="716a2-334">從 *web.config* 檔案本機移除 `<handlerSettings>` 並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-334">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="716a2-335">使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。</span><span class="sxs-lookup"><span data-stu-id="716a2-335">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="716a2-336">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-336">Save the file.</span></span>

<span data-ttu-id="716a2-337">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="716a2-337">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="716a2-338">無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-338">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="716a2-339">記錄檔大小沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="716a2-339">There's no limit on log file size.</span></span> <span data-ttu-id="716a2-340">請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。</span><span class="sxs-lookup"><span data-stu-id="716a2-340">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="716a2-341">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="716a2-341">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="716a2-342">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="716a2-342">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="716a2-343">緩慢或懸掛應用程式 (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="716a2-343">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="716a2-344">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="716a2-344">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="716a2-345">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="716a2-345">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="716a2-346">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="716a2-346">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="716a2-347">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="716a2-347">Monitoring blades</span></span>

<span data-ttu-id="716a2-348">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="716a2-348">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="716a2-349">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-349">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="716a2-350">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="716a2-350">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="716a2-351">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="716a2-351">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="716a2-352">在 [開發工具]  刀鋒視窗區段中，選取 [延伸模組]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="716a2-352">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="716a2-353">[ASP.NET Core 延伸模組]  應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="716a2-353">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="716a2-354">如果未安裝這些延伸模組，請選取 [新增]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-354">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="716a2-355">從清單中選擇 [ASP.NET Core 延伸模組]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-355">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="716a2-356">選取 [確定]  以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="716a2-356">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="716a2-357">在 [新增延伸模組]  刀鋒視窗上，選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-357">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="716a2-358">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="716a2-358">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="716a2-359">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="716a2-359">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="716a2-360">在 Azure 入口網站中，選取 [開發工具]  區域中的 [進階工具]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="716a2-360">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="716a2-361">選取 [執行&rarr;]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-361">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="716a2-362">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="716a2-362">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="716a2-363">使用頁面頂端的導覽列，開啟 [偵錯主控台]  ，然後選取 [CMD]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-363">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="716a2-364">將資料夾開啟至路徑 [site]   > [wwwroot]  ，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-364">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="716a2-365">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="716a2-365">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="716a2-366">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="716a2-366">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="716a2-367">選取 [儲存]  以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-367">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="716a2-368">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="716a2-368">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="716a2-369">在 Azure 入口網站中，選取 [診斷記錄檔]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="716a2-369">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="716a2-370">選取 [應用程式記錄 (檔案系統)]  和 [詳細錯誤訊息]  .的 [開啟]  開關。</span><span class="sxs-lookup"><span data-stu-id="716a2-370">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="716a2-371">選取刀鋒視窗頂端的 [儲存]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="716a2-371">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="716a2-372">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤]  的 [開啟]  開關。</span><span class="sxs-lookup"><span data-stu-id="716a2-372">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="716a2-373">選取 [記錄資料流]  刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔]  刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="716a2-373">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="716a2-374">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="716a2-374">Make a request to the app.</span></span>
1. <span data-ttu-id="716a2-375">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="716a2-375">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="716a2-376">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="716a2-376">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="716a2-377">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="716a2-377">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="716a2-378">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]  刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="716a2-378">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="716a2-379">從資訊看板的 [支援工具]  區域中，選取 [失敗要求追蹤記錄檔]  。</span><span class="sxs-lookup"><span data-stu-id="716a2-379">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="716a2-380">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和 [Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="716a2-380">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="716a2-381">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="716a2-381">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="716a2-382">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-382">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="716a2-383">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="716a2-383">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="716a2-384">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="716a2-384">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="716a2-385">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="716a2-385">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="716a2-386">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="716a2-386">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="716a2-387">應用程式事件記錄檔 (IIS)</span><span class="sxs-lookup"><span data-stu-id="716a2-387">Application Event Log (IIS)</span></span>

<span data-ttu-id="716a2-388">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="716a2-388">Access the Application Event Log:</span></span>

1. <span data-ttu-id="716a2-389">開啟 [開始] 功能表，搜尋**事件檢視器**，然後選取 [事件檢視器]  應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-389">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="716a2-390">在 [事件檢視器]  中，開啟 [Windows 記錄]  節點。</span><span class="sxs-lookup"><span data-stu-id="716a2-390">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="716a2-391">選取 [應用程式]  以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="716a2-391">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="716a2-392">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-392">Search for errors associated with the failing app.</span></span> <span data-ttu-id="716a2-393">錯誤在 [來源]  資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="716a2-393">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="716a2-394">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="716a2-394">Run the app at a command prompt</span></span>

<span data-ttu-id="716a2-395">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="716a2-395">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="716a2-396">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="716a2-396">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="716a2-397">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="716a2-397">Framework-dependent deployment</span></span>

<span data-ttu-id="716a2-398">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="716a2-398">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="716a2-399">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-399">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="716a2-400">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="716a2-400">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="716a2-401">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="716a2-401">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="716a2-402">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="716a2-402">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="716a2-403">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="716a2-403">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="716a2-404">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="716a2-404">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="716a2-405">自封式部署</span><span class="sxs-lookup"><span data-stu-id="716a2-405">Self-contained deployment</span></span>

<span data-ttu-id="716a2-406">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="716a2-406">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="716a2-407">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="716a2-407">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="716a2-408">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="716a2-408">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="716a2-409">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="716a2-409">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="716a2-410">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="716a2-410">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="716a2-411">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="716a2-411">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="716a2-412">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="716a2-412">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="716a2-413">ASP.NET Core 模組 stdout 記錄檔 (IIS)</span><span class="sxs-lookup"><span data-stu-id="716a2-413">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="716a2-414">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="716a2-414">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="716a2-415">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="716a2-415">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="716a2-416">如果 [logs]  資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="716a2-416">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="716a2-417">如需有關如何讓 MSBuild 在部署中自動建立 [logs]  資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="716a2-417">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="716a2-418">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-418">Edit the *web.config* file.</span></span> <span data-ttu-id="716a2-419">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs]  資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="716a2-419">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="716a2-420">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="716a2-420">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="716a2-421">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="716a2-421">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="716a2-422">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="716a2-422">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="716a2-423">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="716a2-423">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="716a2-424">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-424">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="716a2-425">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="716a2-425">Make a request to the app.</span></span>
1. <span data-ttu-id="716a2-426">瀏覽至 [logs]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="716a2-426">Navigate to the *logs* folder.</span></span> <span data-ttu-id="716a2-427">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="716a2-427">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="716a2-428">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="716a2-428">Study the log for errors.</span></span>

<span data-ttu-id="716a2-429">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="716a2-429">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="716a2-430">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-430">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="716a2-431">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="716a2-431">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="716a2-432">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-432">Save the file.</span></span>

<span data-ttu-id="716a2-433">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="716a2-433">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="716a2-434">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-434">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="716a2-435">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="716a2-435">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="716a2-436">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="716a2-436">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="716a2-437">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="716a2-437">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="716a2-438">ASP.NET Core 模組的調試記錄檔 (IIS)</span><span class="sxs-lookup"><span data-stu-id="716a2-438">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="716a2-439">將下列處理常式設定新增至應用程式  的 web.config 檔案, 以啟用 ASP.NET Core 模組的 debug 記錄檔:</span><span class="sxs-lookup"><span data-stu-id="716a2-439">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="716a2-440">確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="716a2-440">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="716a2-441">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="716a2-441">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="716a2-442">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="716a2-442">Enable the Developer Exception Page</span></span>

<span data-ttu-id="716a2-443">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-443">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="716a2-444">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="716a2-444">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="716a2-445">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="716a2-445">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="716a2-446">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="716a2-446">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="716a2-447">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="716a2-447">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="716a2-448">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="716a2-448">Obtain data from an app</span></span>

<span data-ttu-id="716a2-449">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="716a2-449">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="716a2-450">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="716a2-450">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="716a2-451">緩慢或懸掛應用程式 (IIS)</span><span class="sxs-lookup"><span data-stu-id="716a2-451">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="716a2-452">損*毀*傾印是系統記憶體的快照集, 有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="716a2-452">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="716a2-453">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="716a2-453">App crashes or encounters an exception</span></span>

<span data-ttu-id="716a2-454">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="716a2-454">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="716a2-455">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-455">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="716a2-456">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="716a2-456">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="716a2-457">執行 [EnableDumps PowerShell 指令碼](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="716a2-457">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="716a2-458">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="716a2-458">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="716a2-459">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="716a2-459">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="716a2-460">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-460">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="716a2-461">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="716a2-461">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="716a2-462">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="716a2-462">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="716a2-463">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="716a2-463">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="716a2-464">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-464">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="716a2-465">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="716a2-465">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="716a2-466">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="716a2-466">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="716a2-467">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="716a2-467">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="716a2-468">當應用程式「停止回應」  (停止回應但未損毀)、在啟動期間失敗，或正常執行時，請參閱[使用者模式傾印檔案：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)，選取適當的工具來產生傾印。</span><span class="sxs-lookup"><span data-stu-id="716a2-468">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="716a2-469">分析傾印</span><span class="sxs-lookup"><span data-stu-id="716a2-469">Analyze the dump</span></span>

<span data-ttu-id="716a2-470">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="716a2-470">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="716a2-471">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="716a2-471">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="716a2-472">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="716a2-472">Clear package caches</span></span>

<span data-ttu-id="716a2-473">有時候, 正常運作的應用程式會在升級開發電腦上的 .NET Core SDK, 或變更應用程式內的套件版本之後立即失敗。</span><span class="sxs-lookup"><span data-stu-id="716a2-473">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="716a2-474">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="716a2-474">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="716a2-475">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="716a2-475">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="716a2-476">刪除 [bin]  和 [obj]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="716a2-476">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="716a2-477">從命令 shell 執行`dotnet nuget locals all --clear` , 以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="716a2-477">Clear the package caches by executing `dotnet nuget locals all --clear` from a command shell.</span></span>

   <span data-ttu-id="716a2-478">清除套件快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成, 並執行命令`nuget locals all -clear`。</span><span class="sxs-lookup"><span data-stu-id="716a2-478">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="716a2-479">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="716a2-479">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="716a2-480">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="716a2-480">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="716a2-481">重新部署應用程式之前, 請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="716a2-481">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="716a2-482">其他資源</span><span class="sxs-lookup"><span data-stu-id="716a2-482">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="716a2-483">Azure 檔</span><span class="sxs-lookup"><span data-stu-id="716a2-483">Azure documentation</span></span>

* [<span data-ttu-id="716a2-484">Application Insights for ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="716a2-484">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="716a2-485">使用 Visual Studio 在 Azure App Service 中疑難排解 web 應用程式的遠端偵錯程式一節</span><span class="sxs-lookup"><span data-stu-id="716a2-485">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="716a2-486">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="716a2-486">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="716a2-487">如何：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="716a2-487">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="716a2-488">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="716a2-488">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="716a2-489">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="716a2-489">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="716a2-490">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="716a2-490">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="716a2-491">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="716a2-491">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="716a2-492">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="716a2-492">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="716a2-493">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="716a2-493">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="716a2-494">Visual Studio 文件</span><span class="sxs-lookup"><span data-stu-id="716a2-494">Visual Studio documentation</span></span>

* [<span data-ttu-id="716a2-495">Visual Studio 2017 中 Azure 上 IIS 的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="716a2-495">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="716a2-496">Visual Studio 2017 中遠端 IIS 電腦上的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="716a2-496">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="716a2-497">了解使用 Visual Studio 進行偵錯</span><span class="sxs-lookup"><span data-stu-id="716a2-497">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="716a2-498">Visual Studio Code 檔</span><span class="sxs-lookup"><span data-stu-id="716a2-498">Visual Studio Code documentation</span></span>

* <span data-ttu-id="716a2-499">[使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="716a2-499">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)</span></span>
