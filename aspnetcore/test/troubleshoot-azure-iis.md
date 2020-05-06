---
title: 疑難排解 Azure App Service 和 IIS 上的 ASP.NET Core
author: rick-anderson
description: 瞭解如何診斷 ASP.NET Core 應用程式的 Azure App Service 和 Internet Information Services （IIS）部署問題。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 09b004abd423abc9cc8e83d3bb3fea1dddf09e14
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776626"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="b5027-103">疑難排解 Azure App Service 和 IIS 上的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5027-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="b5027-104">作者：[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="b5027-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b5027-105">本文提供有關一般應用程式啟動錯誤的資訊，以及如何在將應用程式部署至 Azure App Service 或 IIS 時診斷錯誤的指示：</span><span class="sxs-lookup"><span data-stu-id="b5027-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="b5027-106">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="b5027-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="b5027-107">說明常見的啟動 HTTP 狀態碼案例。</span><span class="sxs-lookup"><span data-stu-id="b5027-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="b5027-108">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="b5027-109">提供部署至 Azure App Service 之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="b5027-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="b5027-110">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="b5027-111">提供部署至 IIS 或在本機 IIS Express 上執行之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="b5027-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="b5027-112">本指南適用于 Windows Server 和 Windows 桌面部署。</span><span class="sxs-lookup"><span data-stu-id="b5027-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="b5027-113">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="b5027-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="b5027-114">說明當一致封裝在執行主要升級或變更封裝版本時，中斷應用程式時該怎麼辦。</span><span class="sxs-lookup"><span data-stu-id="b5027-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="b5027-115">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5027-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="b5027-116">列出其他疑難排解主題。</span><span class="sxs-lookup"><span data-stu-id="b5027-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="b5027-117">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="b5027-117">App startup errors</span></span>

<span data-ttu-id="b5027-118">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="b5027-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="b5027-119">使用本主題中的建議，可以診斷在本機進行偵錯工具時所發生的*502.5-進程失敗*或*500.30 啟動失敗*。</span><span class="sxs-lookup"><span data-stu-id="b5027-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="b5027-120">403.14 禁止</span><span class="sxs-lookup"><span data-stu-id="b5027-120">403.14 Forbidden</span></span>

<span data-ttu-id="b5027-121">應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-121">The app fails to start.</span></span> <span data-ttu-id="b5027-122">會記錄下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="b5027-122">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="b5027-123">此錯誤通常是因為主控系統上的部署中斷所造成，其中包括下列任何一種情況：</span><span class="sxs-lookup"><span data-stu-id="b5027-123">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="b5027-124">應用程式會部署到裝載系統上錯誤的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-124">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="b5027-125">部署進程無法將應用程式的所有檔案和資料夾移到主控系統上的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-125">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="b5027-126">部署中遺漏*web.config*檔案，或*web.config*檔案內容的格式不正確。</span><span class="sxs-lookup"><span data-stu-id="b5027-126">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="b5027-127">執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b5027-127">Perform the following steps:</span></span>

1. <span data-ttu-id="b5027-128">刪除主控系統上部署資料夾中的所有檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-128">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="b5027-129">使用一般部署方法（例如 Visual Studio、PowerShell 或手動部署），將應用程式的 [*發佈*] 資料夾的內容重新部署至主機系統：</span><span class="sxs-lookup"><span data-stu-id="b5027-129">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="b5027-130">請確認*web.config*檔案存在於部署中，而且其內容正確。</span><span class="sxs-lookup"><span data-stu-id="b5027-130">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="b5027-131">在 Azure App Service 上裝載時，請確認應用程式已部署至`D:\home\site\wwwroot`資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-131">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="b5027-132">當應用程式由 IIS 裝載時，請確認應用程式已部署到 iis**管理員**的 [**基本設定**] 中所顯示的 iis**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="b5027-132">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="b5027-133">藉由比較主控系統上的部署與專案 [*發行*] 資料夾的內容，確認已部署所有應用程式的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-133">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="b5027-134">如需已發佈之 ASP.NET Core 應用程式佈建的詳細資訊， <xref:host-and-deploy/directory-structure>請參閱。</span><span class="sxs-lookup"><span data-stu-id="b5027-134">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="b5027-135">如需*web.config*檔案的詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="b5027-135">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="b5027-136">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="b5027-136">500 Internal Server Error</span></span>

<span data-ttu-id="b5027-137">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-137">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="b5027-138">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="b5027-138">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="b5027-139">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」\*\* 的形式出現。</span><span class="sxs-lookup"><span data-stu-id="b5027-139">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="b5027-140">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-140">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="b5027-141">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="b5027-141">From the server's perspective, that's correct.</span></span> <span data-ttu-id="b5027-142">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="b5027-142">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="b5027-143">請在伺服器上於命令提示字元中執行應用程式或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="b5027-143">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="b5027-144">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="b5027-144">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="b5027-145">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-145">The worker process fails.</span></span> <span data-ttu-id="b5027-146">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-146">The app doesn't start.</span></span>

<span data-ttu-id="b5027-147">載入[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)元件時發生未知的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-147">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="b5027-148">請採取下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="b5027-148">Take one of the following actions:</span></span>

* <span data-ttu-id="b5027-149">連絡 [Microsoft 支援服務](https://support.microsoft.com/oas/default.aspx?prid=15832) (依序選取 [開發人員工具]\*\*\*\* 和 [ASP.NET Core]\*\*\*\*)。</span><span class="sxs-lookup"><span data-stu-id="b5027-149">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="b5027-150">在 Stack Overflow 上詢問問題。</span><span class="sxs-lookup"><span data-stu-id="b5027-150">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="b5027-151">在我們的 [GitHub 存放庫](https://github.com/dotnet/AspNetCore)提出問題。</span><span class="sxs-lookup"><span data-stu-id="b5027-151">File an issue on our [GitHub repository](https://github.com/dotnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="b5027-152">500.30 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="b5027-152">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="b5027-153">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-153">The worker process fails.</span></span> <span data-ttu-id="b5027-154">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-154">The app doesn't start.</span></span>

<span data-ttu-id="b5027-155">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動 .NET Core CLR 同進程，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-155">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="b5027-156">通常從應用程式事件記錄檔和 ASP.NET Core 模組 stdout 記錄檔中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-156">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="b5027-157">常見的失敗狀況：</span><span class="sxs-lookup"><span data-stu-id="b5027-157">Common failure conditions:</span></span>

* <span data-ttu-id="b5027-158">應用程式設定錯誤，因為以不存在的 ASP.NET Core 共用架構版本為目標。</span><span class="sxs-lookup"><span data-stu-id="b5027-158">The app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="b5027-159">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="b5027-159">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>
* <span data-ttu-id="b5027-160">使用 Azure Key Vault，缺少 Key Vault 的許可權。</span><span class="sxs-lookup"><span data-stu-id="b5027-160">Using Azure Key Vault, lack of permissions to the Key Vault.</span></span> <span data-ttu-id="b5027-161">檢查目標 Key Vault 中的存取原則，以確定已授與正確的許可權。</span><span class="sxs-lookup"><span data-stu-id="b5027-161">Check the access policies in the targeted Key Vault to ensure that the correct permissions are granted.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="b5027-162">500.31 ANCM 找不到原生相依性</span><span class="sxs-lookup"><span data-stu-id="b5027-162">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="b5027-163">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-163">The worker process fails.</span></span> <span data-ttu-id="b5027-164">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-164">The app doesn't start.</span></span>

<span data-ttu-id="b5027-165">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試在同進程中啟動 .net Core 執行時間，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-165">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="b5027-166">此啟動失敗的最常見原因是當 `Microsoft.NETCore.App` 或 `Microsoft.AspNetCore.App` 執行階段未安裝時。</span><span class="sxs-lookup"><span data-stu-id="b5027-166">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="b5027-167">如果應用程式部署至目標 ASP.NET Core 3.0，但電腦上無該版本，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-167">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="b5027-168">範例錯誤訊息如下：</span><span class="sxs-lookup"><span data-stu-id="b5027-168">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="b5027-169">錯誤訊息會列出所有已安裝 .NET Core 版本和應用程式所要求的版本。</span><span class="sxs-lookup"><span data-stu-id="b5027-169">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="b5027-170">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="b5027-170">To fix this error, either:</span></span>

* <span data-ttu-id="b5027-171">在電腦上安裝適當的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="b5027-171">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="b5027-172">將應用程式的目標 .NET Core 版本變更為電腦上版本。</span><span class="sxs-lookup"><span data-stu-id="b5027-172">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="b5027-173">將應用程式發佈為[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="b5027-173">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="b5027-174">在開發過程中執行時 (`ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`)，特定的錯誤會寫入至 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="b5027-174">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="b5027-175">處理序啟動失敗的原因也會列在應用程式事件記錄檔中。</span><span class="sxs-lookup"><span data-stu-id="b5027-175">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="b5027-176">500.32 ANCM 無法載入 dll</span><span class="sxs-lookup"><span data-stu-id="b5027-176">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="b5027-177">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-177">The worker process fails.</span></span> <span data-ttu-id="b5027-178">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-178">The app doesn't start.</span></span>

<span data-ttu-id="b5027-179">此錯誤最常見原因是針對不相容的處理器架構發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-179">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="b5027-180">如果背景工作處理序執行為 32 位元應用程式，而此應用程式已發佈至目標 64 位元，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-180">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="b5027-181">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="b5027-181">To fix this error, either:</span></span>

* <span data-ttu-id="b5027-182">針對相同的處理器架構，將應用程式重新發佈為背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="b5027-182">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="b5027-183">將應用程式發佈為[架構相依部署](/dotnet/core/deploying/#framework-dependent-executables-fde)。</span><span class="sxs-lookup"><span data-stu-id="b5027-183">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="b5027-184">500.33 ANCM 要求處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="b5027-184">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="b5027-185">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-185">The worker process fails.</span></span> <span data-ttu-id="b5027-186">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-186">The app doesn't start.</span></span>

<span data-ttu-id="b5027-187">應用程式未參考 `Microsoft.AspNetCore.App` 架構。</span><span class="sxs-lookup"><span data-stu-id="b5027-187">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="b5027-188">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)只能主控`Microsoft.AspNetCore.App`以架構為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-188">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="b5027-189">若要修正這個錯誤，請確認應用程式以 `Microsoft.AspNetCore.App` 架構為目標。</span><span class="sxs-lookup"><span data-stu-id="b5027-189">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="b5027-190">檢查 `.runtimeconfig.json` 以驗證應用程式是否以該架構為目標。</span><span class="sxs-lookup"><span data-stu-id="b5027-190">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="b5027-191">500.34 ANCM 不支援混合式裝載模型</span><span class="sxs-lookup"><span data-stu-id="b5027-191">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="b5027-192">背景工作處理序無法在相同的程序中執行同處理序應用程式和跨處理序應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-192">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="b5027-193">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-193">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="b5027-194">500.35 ANCM 同一程序中有多個同處理序應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-194">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="b5027-195">工作者進程無法在同一個進程中執行多個同進程應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-195">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="b5027-196">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-196">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="b5027-197">500.36 ANCM 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="b5027-197">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="b5027-198">跨處理序要求處理常式 *aspnetcorev2_outofprocess.dll* 不在 *aspnetcorev2.dll* 檔案旁邊。</span><span class="sxs-lookup"><span data-stu-id="b5027-198">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="b5027-199">這表示[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)安裝損毀。</span><span class="sxs-lookup"><span data-stu-id="b5027-199">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="b5027-200">若要修正這個錯誤，請修復 [.NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (適用於 IIS) 或 Visual Studio (適用於 IIS Express) 安裝。</span><span class="sxs-lookup"><span data-stu-id="b5027-200">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="b5027-201">500.37 ANCM 無法在啟動時間限制內啟動</span><span class="sxs-lookup"><span data-stu-id="b5027-201">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="b5027-202">ANCM 無法在提供的啟動時間限制內啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-202">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="b5027-203">根據預設，逾時值為 120 秒。</span><span class="sxs-lookup"><span data-stu-id="b5027-203">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="b5027-204">在同一部電腦上啟動大量的應用程式時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-204">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="b5027-205">檢查伺服器在啟動期間是否出現 CPU/記憶體的使用量尖峰。</span><span class="sxs-lookup"><span data-stu-id="b5027-205">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="b5027-206">多個應用程式的啟動程序可能需要交錯進行。</span><span class="sxs-lookup"><span data-stu-id="b5027-206">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="50038-ancm-application-dll-not-found"></a><span data-ttu-id="b5027-207">500.38 找不到 ANCM 應用程式 DLL</span><span class="sxs-lookup"><span data-stu-id="b5027-207">500.38 ANCM Application DLL Not Found</span></span>

<span data-ttu-id="b5027-208">ANCM 找不到應用程式 DLL，其應該位於可執行檔的旁邊。</span><span class="sxs-lookup"><span data-stu-id="b5027-208">ANCM failed to locate the application DLL, which should be next to the executable.</span></span>

<span data-ttu-id="b5027-209">使用同進程裝載模型來裝載封裝為[單一檔案可執行](/dotnet/core/whats-new/dotnet-core-3-0#single-file-executables)檔的應用程式時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-209">This error occurs when hosting an app packaged as a [single-file executable](/dotnet/core/whats-new/dotnet-core-3-0#single-file-executables) using the in-process hosting model.</span></span> <span data-ttu-id="b5027-210">同進程模型要求 ANCM 將 .NET Core 應用程式載入至現有的 IIS 進程。</span><span class="sxs-lookup"><span data-stu-id="b5027-210">The in-process model requires that the ANCM load the .NET Core app into the existing IIS process.</span></span> <span data-ttu-id="b5027-211">單一檔案部署模型不支援此案例。</span><span class="sxs-lookup"><span data-stu-id="b5027-211">This scenario isn't supported by the single-file deployment model.</span></span> <span data-ttu-id="b5027-212">在應用程式的專案檔中，使用下列**其中一**種方法來修正此錯誤：</span><span class="sxs-lookup"><span data-stu-id="b5027-212">Use **one** of the following approaches in the app's project file to fix this error:</span></span>

1. <span data-ttu-id="b5027-213">將`PublishSingleFile` MSBuild 屬性設定為，以`false`停用單一檔案發行。</span><span class="sxs-lookup"><span data-stu-id="b5027-213">Disable single-file publishing by setting the `PublishSingleFile` MSBuild property to `false`.</span></span>
1. <span data-ttu-id="b5027-214">將`AspNetCoreHostingModel` MSBuild 屬性設定為`OutOfProcess`，以切換至跨進程裝載模型。</span><span class="sxs-lookup"><span data-stu-id="b5027-214">Switch to the out-of-process hosting model by setting the `AspNetCoreHostingModel` MSBuild property to `OutOfProcess`.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="b5027-215">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="b5027-215">502.5 Process Failure</span></span>

<span data-ttu-id="b5027-216">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-216">The worker process fails.</span></span> <span data-ttu-id="b5027-217">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-217">The app doesn't start.</span></span>

<span data-ttu-id="b5027-218">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-218">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="b5027-219">通常從應用程式事件記錄檔和 ASP.NET Core 模組 stdout 記錄檔中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-219">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="b5027-220">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="b5027-220">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="b5027-221">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="b5027-221">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="b5027-222">「共用的架構」\*\* 是一組安裝於電腦上且由 `Microsoft.AspNetCore.App` 之類的中繼套件所參考的組件 (*.dll* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="b5027-222">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="b5027-223">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="b5027-223">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="b5027-224">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="b5027-224">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="b5027-225">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗]\*\* 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="b5027-225">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="b5027-226">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="b5027-226">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="b5027-227">應用程式無法啟動，因為無法載入應用程式的組件 (*.dll*)。</span><span class="sxs-lookup"><span data-stu-id="b5027-227">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="b5027-228">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-228">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="b5027-229">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="b5027-229">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="b5027-230">在 IIS 管理員的 [應用程式集區]\*\*\*\* 中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="b5027-230">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="b5027-231">在 [動作]\*\*\*\* 面板中，選取 [編輯應用程式集區]\*\*\*\* 下的 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-231">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="b5027-232">設定 [啟用 32 位元應用程式]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="b5027-232">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="b5027-233">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="b5027-233">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="b5027-234">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="b5027-234">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="b5027-235">確認專案檔中的`<Platform>` MSBuild 屬性和應用程式的已發佈位之間沒有衝突。</span><span class="sxs-lookup"><span data-stu-id="b5027-235">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="b5027-236">連線重設</span><span class="sxs-lookup"><span data-stu-id="b5027-236">Connection reset</span></span>

<span data-ttu-id="b5027-237">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-237">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="b5027-238">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-238">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="b5027-239">這類錯誤會在用戶端上顯示為「連線重設」\*\* 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-239">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="b5027-240">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="b5027-240">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="b5027-241">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="b5027-241">Default startup limits</span></span>

<span data-ttu-id="b5027-242">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是以120秒的預設*startupTimeLimit*來設定。</span><span class="sxs-lookup"><span data-stu-id="b5027-242">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="b5027-243">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-243">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="b5027-244">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="b5027-244">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="b5027-245">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-245">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="b5027-246">應用程式事件記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-246">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="b5027-247">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題]\*\*\*\* 刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="b5027-247">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="b5027-248">在 Azure 入口網站的 [應用程式服務]\*\*\*\* 中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-248">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="b5027-249">選取 [診斷並解決問題]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-249">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="b5027-250">選取 [診斷工具]\*\*\*\* 標題。</span><span class="sxs-lookup"><span data-stu-id="b5027-250">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="b5027-251">在 [支援工具]\*\*\*\* 下，選取 [應用程式事件]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-251">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="b5027-252">檢查 [來源]\*\*\*\* 資料行中 *IIS AspNetCoreModule* 或 *IIS AspNetCoreModule V2* 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-252">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="b5027-253">除了使用 [診斷並解決問題]\*\*\*\* 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="b5027-253">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="b5027-254">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-254">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b5027-255">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-255">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-256">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-256">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-257">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-257">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-258">開啟 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-258">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="b5027-259">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b5027-259">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="b5027-260">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-260">Examine the log.</span></span> <span data-ttu-id="b5027-261">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="b5027-261">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="b5027-262">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-262">Run the app in the Kudu console</span></span>

<span data-ttu-id="b5027-263">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="b5027-263">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="b5027-264">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="b5027-264">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="b5027-265">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-265">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b5027-266">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-266">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-267">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-267">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-268">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-268">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="b5027-269">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-269">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="b5027-270">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="b5027-270">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="b5027-271">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="b5027-271">Run the app:</span></span>
   * <span data-ttu-id="b5027-272">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-272">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="b5027-273">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-273">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="b5027-274">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-274">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="b5027-275">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="b5027-275">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="b5027-276">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="b5027-276">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="b5027-277">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="b5027-277">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="b5027-278">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b5027-278">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="b5027-279">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-279">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="b5027-280">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-280">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="b5027-281">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="b5027-281">**Current release**</span></span>

* <span data-ttu-id="b5027-282">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-282">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="b5027-283">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b5027-283">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="b5027-284">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-284">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="b5027-285">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="b5027-285">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="b5027-286">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-286">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="b5027-287">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="b5027-287">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="b5027-288">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="b5027-288">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="b5027-289">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="b5027-289">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="b5027-290">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b5027-290">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="b5027-291">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-291">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="b5027-292">ASP.NET Core 模組 stdout 記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-292">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="b5027-293">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="b5027-293">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="b5027-294">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b5027-294">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b5027-295">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-295">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="b5027-296">在 [選取問題類別]\*\*\*\* 底下，選取 [Web 應用程式停止運作]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-296">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="b5027-297">在 [建議的解決方案]**[啟用 Stdout 記錄檔重新導向]** > \*\*\*\* 底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-297">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="b5027-298">在 Kudu [診斷主控台]\*\*\*\* 中，將資料夾開啟至路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-298">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b5027-299">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-299">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="b5027-300">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b5027-300">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="b5027-301">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="b5027-301">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="b5027-302">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-302">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="b5027-303">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-303">Make a request to the app.</span></span>
1. <span data-ttu-id="b5027-304">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="b5027-304">Return to the Azure portal.</span></span> <span data-ttu-id="b5027-305">選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-305">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="b5027-306">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-306">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-307">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-307">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-308">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-308">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-309">選取 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-309">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="b5027-310">檢查 [修改時間]\*\*\*\* 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-310">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="b5027-311">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-311">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="b5027-312">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-312">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="b5027-313">在 Kudu [診斷主控台]\*\*\*\* 中，返回路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\* 以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-313">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="b5027-314">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-314">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="b5027-315">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="b5027-315">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="b5027-316">選取 [儲存]\*\*\*\* 以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-316">Select **Save** to save the file.</span></span>

<span data-ttu-id="b5027-317">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="b5027-317">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-318">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-318">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-319">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-319">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="b5027-320">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-320">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="b5027-321">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-321">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-322">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-322">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="b5027-323">ASP.NET Core 模組的 debug 記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-323">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="b5027-324">ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-324">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="b5027-325">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b5027-325">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b5027-326">若要啟用增強型診斷記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="b5027-326">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="b5027-327">遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-327">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="b5027-328">重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-328">Redeploy the app.</span></span>
   * <span data-ttu-id="b5027-329">使用 Kudu 主控台，將[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所顯示的 `<handlerSettings>` 新增至即時應用程式的 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="b5027-329">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="b5027-330">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-330">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b5027-331">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-331">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-332">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-332">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="b5027-333">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-333">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="b5027-334">開啟路徑**site** > **wwwroot**的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-334">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b5027-335">選取鉛筆圖示來編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-335">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="b5027-336">新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。</span><span class="sxs-lookup"><span data-stu-id="b5027-336">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="b5027-337">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-337">Select the **Save** button.</span></span>
1. <span data-ttu-id="b5027-338">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-338">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b5027-339">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-339">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-340">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-340">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-341">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-341">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-342">開啟路徑**site** > **wwwroot**的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-342">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b5027-343">如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。</span><span class="sxs-lookup"><span data-stu-id="b5027-343">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="b5027-344">如果已提供路徑，請巡覽至記錄檔的位置。</span><span class="sxs-lookup"><span data-stu-id="b5027-344">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="b5027-345">使用檔案名稱旁的鉛筆圖示來開啟記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-345">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="b5027-346">完成疑難排解時，請停用偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-346">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="b5027-347">若要停用增強型偵錯記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="b5027-347">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="b5027-348">從 *web.config* 檔案本機移除 `<handlerSettings>` 並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-348">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="b5027-349">使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。</span><span class="sxs-lookup"><span data-stu-id="b5027-349">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="b5027-350">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-350">Save the file.</span></span>

<span data-ttu-id="b5027-351">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="b5027-351">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-352">無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-352">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-353">記錄檔大小沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-353">There's no limit on log file size.</span></span> <span data-ttu-id="b5027-354">請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-354">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="b5027-355">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-355">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-356">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-356">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="b5027-357">緩慢或懸掛應用程式（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-357">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="b5027-358">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="b5027-358">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="b5027-359">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-359">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="b5027-360">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b5027-360">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="b5027-361">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="b5027-361">Monitoring blades</span></span>

<span data-ttu-id="b5027-362">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="b5027-362">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="b5027-363">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-363">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="b5027-364">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="b5027-364">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="b5027-365">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="b5027-365">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="b5027-366">在 [開發工具]\*\*\*\* 刀鋒視窗區段中，選取 [延伸模組]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-366">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="b5027-367">[ASP.NET Core 延伸模組]\*\*\*\* 應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="b5027-367">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="b5027-368">如果未安裝這些延伸模組，請選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-368">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="b5027-369">從清單中選擇 [ASP.NET Core 延伸模組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-369">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="b5027-370">選取 [確定]\*\*\*\* 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="b5027-370">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="b5027-371">在 [新增延伸模組]\*\*\*\* 刀鋒視窗上，選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-371">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="b5027-372">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="b5027-372">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="b5027-373">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="b5027-373">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="b5027-374">在 Azure 入口網站中，選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-374">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="b5027-375">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-375">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-376">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-376">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-377">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-377">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-378">將資料夾開啟至路徑 [site]**[wwwroot]** > \*\*\*\*，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-378">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="b5027-379">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b5027-379">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="b5027-380">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="b5027-380">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="b5027-381">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-381">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="b5027-382">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-382">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="b5027-383">在 Azure 入口網站中，選取 [診斷記錄檔]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-383">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="b5027-384">選取 [應用程式記錄 (檔案系統)]\*\*\*\* 和 [詳細錯誤訊息]\*\*\*\*.的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="b5027-384">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="b5027-385">選取刀鋒視窗頂端的 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-385">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="b5027-386">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤]\*\*\*\* 的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="b5027-386">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="b5027-387">選取 [記錄資料流]\*\*\*\* 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔]\*\*\*\* 刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="b5027-387">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="b5027-388">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-388">Make a request to the app.</span></span>
1. <span data-ttu-id="b5027-389">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-389">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="b5027-390">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-390">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="b5027-391">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="b5027-391">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="b5027-392">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-392">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="b5027-393">從資訊看板的 [支援工具]\*\*\*\* 區域中，選取 [失敗要求追蹤記錄檔]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-393">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="b5027-394">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="b5027-394">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="b5027-395">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="b5027-395">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-396">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-396">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-397">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-397">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="b5027-398">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-398">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-399">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-399">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="b5027-400">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-400">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="b5027-401">應用程式事件記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-401">Application Event Log (IIS)</span></span>

<span data-ttu-id="b5027-402">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="b5027-402">Access the Application Event Log:</span></span>

1. <span data-ttu-id="b5027-403">開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-403">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="b5027-404">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="b5027-404">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="b5027-405">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="b5027-405">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="b5027-406">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-406">Search for errors associated with the failing app.</span></span> <span data-ttu-id="b5027-407">錯誤在 [來源]\*\* 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="b5027-407">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="b5027-408">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-408">Run the app at a command prompt</span></span>

<span data-ttu-id="b5027-409">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="b5027-409">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="b5027-410">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-410">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="b5027-411">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="b5027-411">Framework-dependent deployment</span></span>

<span data-ttu-id="b5027-412">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-412">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="b5027-413">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-413">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="b5027-414">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="b5027-414">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="b5027-415">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-415">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="b5027-416">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-416">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="b5027-417">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-417">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="b5027-418">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="b5027-418">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="b5027-419">自封式部署</span><span class="sxs-lookup"><span data-stu-id="b5027-419">Self-contained deployment</span></span>

<span data-ttu-id="b5027-420">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-420">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="b5027-421">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-421">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="b5027-422">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="b5027-422">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="b5027-423">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-423">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="b5027-424">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-424">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="b5027-425">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-425">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="b5027-426">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="b5027-426">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="b5027-427">ASP.NET Core 模組 stdout 記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-427">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="b5027-428">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b5027-428">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b5027-429">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-429">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="b5027-430">如果 [logs]\*\* 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-430">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="b5027-431">如需有關如何讓 MSBuild 在部署中自動建立 [logs]\*\* 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="b5027-431">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="b5027-432">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-432">Edit the *web.config* file.</span></span> <span data-ttu-id="b5027-433">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs]\*\* 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="b5027-433">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="b5027-434">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="b5027-434">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="b5027-435">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="b5027-435">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="b5027-436">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="b5027-436">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="b5027-437">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b5027-437">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="b5027-438">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-438">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="b5027-439">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-439">Make a request to the app.</span></span>
1. <span data-ttu-id="b5027-440">瀏覽至 [logs]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-440">Navigate to the *logs* folder.</span></span> <span data-ttu-id="b5027-441">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-441">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="b5027-442">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-442">Study the log for errors.</span></span>

<span data-ttu-id="b5027-443">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-443">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="b5027-444">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-444">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="b5027-445">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="b5027-445">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="b5027-446">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-446">Save the file.</span></span>

<span data-ttu-id="b5027-447">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="b5027-447">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-448">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-448">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-449">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-449">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="b5027-450">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-450">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-451">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-451">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="b5027-452">ASP.NET Core 模組的調試記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-452">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="b5027-453">將下列處理常式設定新增至應用程式*的 web.config 檔案*，以啟用 ASP.NET Core 模組的 debug 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b5027-453">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="b5027-454">確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b5027-454">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="b5027-455">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="b5027-455">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="b5027-456">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="b5027-456">Enable the Developer Exception Page</span></span>

<span data-ttu-id="b5027-457">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-457">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="b5027-458">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="b5027-458">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="b5027-459">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="b5027-459">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="b5027-460">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="b5027-460">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="b5027-461">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="b5027-461">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="b5027-462">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="b5027-462">Obtain data from an app</span></span>

<span data-ttu-id="b5027-463">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="b5027-463">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="b5027-464">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="b5027-464">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="b5027-465">緩慢或懸掛應用程式（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-465">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="b5027-466">損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-466">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="b5027-467">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="b5027-467">App crashes or encounters an exception</span></span>

<span data-ttu-id="b5027-468">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="b5027-468">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="b5027-469">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-469">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="b5027-470">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b5027-470">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="b5027-471">執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="b5027-471">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="b5027-472">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-472">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="b5027-473">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-473">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="b5027-474">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-474">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="b5027-475">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="b5027-475">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="b5027-476">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-476">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="b5027-477">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-477">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="b5027-478">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-478">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="b5027-479">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="b5027-479">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-480">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="b5027-480">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="b5027-481">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="b5027-481">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="b5027-482">當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。</span><span class="sxs-lookup"><span data-stu-id="b5027-482">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="b5027-483">分析傾印</span><span class="sxs-lookup"><span data-stu-id="b5027-483">Analyze the dump</span></span>

<span data-ttu-id="b5027-484">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="b5027-484">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="b5027-485">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="b5027-485">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="b5027-486">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="b5027-486">Clear package caches</span></span>

<span data-ttu-id="b5027-487">升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-487">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="b5027-488">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-488">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="b5027-489">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="b5027-489">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="b5027-490">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-490">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="b5027-491">從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="b5027-491">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="b5027-492">清除套件快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成，並執行命令`nuget locals all -clear`。</span><span class="sxs-lookup"><span data-stu-id="b5027-492">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="b5027-493">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="b5027-493">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="b5027-494">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="b5027-494">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="b5027-495">重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-495">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5027-496">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5027-496">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="b5027-497">Azure 文件</span><span class="sxs-lookup"><span data-stu-id="b5027-497">Azure documentation</span></span>

* [<span data-ttu-id="b5027-498">ASP.NET Core 的 Application Insights</span><span class="sxs-lookup"><span data-stu-id="b5027-498">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="b5027-499">使用 Visual Studio 在 Azure App Service 中疑難排解 web 應用程式的遠端偵錯程式一節</span><span class="sxs-lookup"><span data-stu-id="b5027-499">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="b5027-500">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="b5027-500">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="b5027-501">作法：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-501">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="b5027-502">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-502">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="b5027-503">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-503">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="b5027-504">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-504">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="b5027-505">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="b5027-505">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="b5027-506">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b5027-506">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="b5027-507">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="b5027-507">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="b5027-508">Visual Studio 文件</span><span class="sxs-lookup"><span data-stu-id="b5027-508">Visual Studio documentation</span></span>

* [<span data-ttu-id="b5027-509">Visual Studio 2017 中 Azure 上 IIS 的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5027-509">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="b5027-510">Visual Studio 2017 中遠端 IIS 電腦上的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5027-510">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="b5027-511">瞭解如何使用 Visual Studio 進行調試</span><span class="sxs-lookup"><span data-stu-id="b5027-511">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="b5027-512">Visual Studio Code 檔</span><span class="sxs-lookup"><span data-stu-id="b5027-512">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="b5027-513">使用 Visual Studio Code 偵錯</span><span class="sxs-lookup"><span data-stu-id="b5027-513">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="b5027-514">本文提供有關一般應用程式啟動錯誤的資訊，以及如何在將應用程式部署至 Azure App Service 或 IIS 時診斷錯誤的指示：</span><span class="sxs-lookup"><span data-stu-id="b5027-514">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="b5027-515">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="b5027-515">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="b5027-516">說明常見的啟動 HTTP 狀態碼案例。</span><span class="sxs-lookup"><span data-stu-id="b5027-516">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="b5027-517">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-517">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="b5027-518">提供部署至 Azure App Service 之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="b5027-518">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="b5027-519">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-519">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="b5027-520">提供部署至 IIS 或在本機 IIS Express 上執行之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="b5027-520">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="b5027-521">本指南適用于 Windows Server 和 Windows 桌面部署。</span><span class="sxs-lookup"><span data-stu-id="b5027-521">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="b5027-522">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="b5027-522">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="b5027-523">說明當一致封裝在執行主要升級或變更封裝版本時，中斷應用程式時該怎麼辦。</span><span class="sxs-lookup"><span data-stu-id="b5027-523">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="b5027-524">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5027-524">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="b5027-525">列出其他疑難排解主題。</span><span class="sxs-lookup"><span data-stu-id="b5027-525">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="b5027-526">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="b5027-526">App startup errors</span></span>

<span data-ttu-id="b5027-527">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="b5027-527">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="b5027-528">使用本主題中的建議，可以診斷在本機進行偵錯工具時所發生的*502.5-進程失敗*或*500.30 啟動失敗*。</span><span class="sxs-lookup"><span data-stu-id="b5027-528">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="b5027-529">403.14 禁止</span><span class="sxs-lookup"><span data-stu-id="b5027-529">403.14 Forbidden</span></span>

<span data-ttu-id="b5027-530">應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-530">The app fails to start.</span></span> <span data-ttu-id="b5027-531">會記錄下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="b5027-531">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="b5027-532">此錯誤通常是因為主控系統上的部署中斷所造成，其中包括下列任何一種情況：</span><span class="sxs-lookup"><span data-stu-id="b5027-532">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="b5027-533">應用程式會部署到裝載系統上錯誤的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-533">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="b5027-534">部署進程無法將應用程式的所有檔案和資料夾移到主控系統上的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-534">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="b5027-535">部署中遺漏*web.config*檔案，或*web.config*檔案內容的格式不正確。</span><span class="sxs-lookup"><span data-stu-id="b5027-535">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="b5027-536">執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b5027-536">Perform the following steps:</span></span>

1. <span data-ttu-id="b5027-537">刪除主控系統上部署資料夾中的所有檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-537">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="b5027-538">使用一般部署方法（例如 Visual Studio、PowerShell 或手動部署），將應用程式的 [*發佈*] 資料夾的內容重新部署至主機系統：</span><span class="sxs-lookup"><span data-stu-id="b5027-538">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="b5027-539">請確認*web.config*檔案存在於部署中，而且其內容正確。</span><span class="sxs-lookup"><span data-stu-id="b5027-539">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="b5027-540">在 Azure App Service 上裝載時，請確認應用程式已部署至`D:\home\site\wwwroot`資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-540">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="b5027-541">當應用程式由 IIS 裝載時，請確認應用程式已部署到 iis**管理員**的 [**基本設定**] 中所顯示的 iis**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="b5027-541">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="b5027-542">藉由比較主控系統上的部署與專案 [*發行*] 資料夾的內容，確認已部署所有應用程式的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-542">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="b5027-543">如需已發佈之 ASP.NET Core 應用程式佈建的詳細資訊， <xref:host-and-deploy/directory-structure>請參閱。</span><span class="sxs-lookup"><span data-stu-id="b5027-543">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="b5027-544">如需*web.config*檔案的詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="b5027-544">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="b5027-545">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="b5027-545">500 Internal Server Error</span></span>

<span data-ttu-id="b5027-546">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-546">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="b5027-547">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="b5027-547">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="b5027-548">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」\*\* 的形式出現。</span><span class="sxs-lookup"><span data-stu-id="b5027-548">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="b5027-549">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-549">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="b5027-550">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="b5027-550">From the server's perspective, that's correct.</span></span> <span data-ttu-id="b5027-551">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="b5027-551">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="b5027-552">請在伺服器上於命令提示字元中執行應用程式或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="b5027-552">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="b5027-553">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="b5027-553">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="b5027-554">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-554">The worker process fails.</span></span> <span data-ttu-id="b5027-555">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-555">The app doesn't start.</span></span>

<span data-ttu-id="b5027-556">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)找不到 .NET Core CLR，而且找不到同進程要求處理常式（*aspnetcorev2_inprocess .dll*）。</span><span class="sxs-lookup"><span data-stu-id="b5027-556">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="b5027-557">請檢查︰</span><span class="sxs-lookup"><span data-stu-id="b5027-557">Check that:</span></span>

* <span data-ttu-id="b5027-558">應用程式以 [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet 套件或 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)為目標。</span><span class="sxs-lookup"><span data-stu-id="b5027-558">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="b5027-559">應用程式設為目標的 ASP.NET Core 共用架構版本有安裝在目標機器上。</span><span class="sxs-lookup"><span data-stu-id="b5027-559">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="b5027-560">500.0 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="b5027-560">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="b5027-561">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-561">The worker process fails.</span></span> <span data-ttu-id="b5027-562">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-562">The app doesn't start.</span></span>

<span data-ttu-id="b5027-563">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)找不到跨進程主控要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="b5027-563">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="b5027-564">請確定 *aspnetcorev2_outofprocess.dll* 出現在子資料夾中，且位於 *aspnetcorev2.dll* 旁。</span><span class="sxs-lookup"><span data-stu-id="b5027-564">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="b5027-565">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="b5027-565">502.5 Process Failure</span></span>

<span data-ttu-id="b5027-566">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-566">The worker process fails.</span></span> <span data-ttu-id="b5027-567">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-567">The app doesn't start.</span></span>

<span data-ttu-id="b5027-568">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-568">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="b5027-569">通常從應用程式事件記錄檔和 ASP.NET Core 模組 stdout 記錄檔中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-569">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="b5027-570">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="b5027-570">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="b5027-571">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="b5027-571">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="b5027-572">「共用的架構」\*\* 是一組安裝於電腦上且由 `Microsoft.AspNetCore.App` 之類的中繼套件所參考的組件 (*.dll* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="b5027-572">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="b5027-573">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="b5027-573">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="b5027-574">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="b5027-574">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="b5027-575">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗]\*\* 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="b5027-575">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="b5027-576">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="b5027-576">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="b5027-577">應用程式無法啟動，因為無法載入應用程式的組件 (*.dll*)。</span><span class="sxs-lookup"><span data-stu-id="b5027-577">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="b5027-578">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-578">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="b5027-579">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="b5027-579">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="b5027-580">在 IIS 管理員的 [應用程式集區]\*\*\*\* 中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="b5027-580">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="b5027-581">在 [動作]\*\*\*\* 面板中，選取 [編輯應用程式集區]\*\*\*\* 下的 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-581">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="b5027-582">設定 [啟用 32 位元應用程式]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="b5027-582">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="b5027-583">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="b5027-583">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="b5027-584">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="b5027-584">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="b5027-585">確認專案檔中的`<Platform>` MSBuild 屬性和應用程式的已發佈位之間沒有衝突。</span><span class="sxs-lookup"><span data-stu-id="b5027-585">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="b5027-586">連線重設</span><span class="sxs-lookup"><span data-stu-id="b5027-586">Connection reset</span></span>

<span data-ttu-id="b5027-587">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-587">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="b5027-588">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-588">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="b5027-589">這類錯誤會在用戶端上顯示為「連線重設」\*\* 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-589">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="b5027-590">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="b5027-590">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="b5027-591">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="b5027-591">Default startup limits</span></span>

<span data-ttu-id="b5027-592">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是以120秒的預設*startupTimeLimit*來設定。</span><span class="sxs-lookup"><span data-stu-id="b5027-592">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="b5027-593">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-593">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="b5027-594">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="b5027-594">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="b5027-595">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-595">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="b5027-596">應用程式事件記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-596">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="b5027-597">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題]\*\*\*\* 刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="b5027-597">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="b5027-598">在 Azure 入口網站的 [應用程式服務]\*\*\*\* 中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-598">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="b5027-599">選取 [診斷並解決問題]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-599">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="b5027-600">選取 [診斷工具]\*\*\*\* 標題。</span><span class="sxs-lookup"><span data-stu-id="b5027-600">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="b5027-601">在 [支援工具]\*\*\*\* 下，選取 [應用程式事件]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-601">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="b5027-602">檢查 [來源]\*\*\*\* 資料行中 *IIS AspNetCoreModule* 或 *IIS AspNetCoreModule V2* 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-602">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="b5027-603">除了使用 [診斷並解決問題]\*\*\*\* 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="b5027-603">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="b5027-604">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-604">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b5027-605">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-605">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-606">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-606">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-607">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-607">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-608">開啟 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-608">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="b5027-609">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b5027-609">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="b5027-610">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-610">Examine the log.</span></span> <span data-ttu-id="b5027-611">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="b5027-611">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="b5027-612">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-612">Run the app in the Kudu console</span></span>

<span data-ttu-id="b5027-613">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="b5027-613">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="b5027-614">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="b5027-614">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="b5027-615">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-615">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b5027-616">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-616">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-617">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-617">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-618">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-618">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="b5027-619">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-619">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="b5027-620">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="b5027-620">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="b5027-621">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="b5027-621">Run the app:</span></span>
   * <span data-ttu-id="b5027-622">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-622">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="b5027-623">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-623">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="b5027-624">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-624">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="b5027-625">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="b5027-625">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="b5027-626">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="b5027-626">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="b5027-627">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="b5027-627">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="b5027-628">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b5027-628">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="b5027-629">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-629">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="b5027-630">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-630">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="b5027-631">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="b5027-631">**Current release**</span></span>

* <span data-ttu-id="b5027-632">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-632">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="b5027-633">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b5027-633">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="b5027-634">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-634">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="b5027-635">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="b5027-635">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="b5027-636">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-636">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="b5027-637">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="b5027-637">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="b5027-638">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="b5027-638">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="b5027-639">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="b5027-639">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="b5027-640">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b5027-640">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="b5027-641">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-641">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="b5027-642">ASP.NET Core 模組 stdout 記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-642">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="b5027-643">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="b5027-643">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="b5027-644">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b5027-644">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b5027-645">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-645">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="b5027-646">在 [選取問題類別]\*\*\*\* 底下，選取 [Web 應用程式停止運作]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-646">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="b5027-647">在 [建議的解決方案]**[啟用 Stdout 記錄檔重新導向]** > \*\*\*\* 底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-647">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="b5027-648">在 Kudu [診斷主控台]\*\*\*\* 中，將資料夾開啟至路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-648">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b5027-649">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-649">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="b5027-650">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b5027-650">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="b5027-651">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="b5027-651">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="b5027-652">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-652">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="b5027-653">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-653">Make a request to the app.</span></span>
1. <span data-ttu-id="b5027-654">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="b5027-654">Return to the Azure portal.</span></span> <span data-ttu-id="b5027-655">選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-655">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="b5027-656">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-656">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-657">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-657">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-658">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-658">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-659">選取 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-659">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="b5027-660">檢查 [修改時間]\*\*\*\* 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-660">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="b5027-661">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-661">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="b5027-662">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-662">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="b5027-663">在 Kudu [診斷主控台]\*\*\*\* 中，返回路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\* 以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-663">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="b5027-664">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-664">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="b5027-665">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="b5027-665">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="b5027-666">選取 [儲存]\*\*\*\* 以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-666">Select **Save** to save the file.</span></span>

<span data-ttu-id="b5027-667">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="b5027-667">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-668">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-668">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-669">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-669">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="b5027-670">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-670">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="b5027-671">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-671">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-672">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-672">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="b5027-673">ASP.NET Core 模組的 debug 記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-673">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="b5027-674">ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-674">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="b5027-675">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b5027-675">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b5027-676">若要啟用增強型診斷記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="b5027-676">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="b5027-677">遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-677">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="b5027-678">重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-678">Redeploy the app.</span></span>
   * <span data-ttu-id="b5027-679">使用 Kudu 主控台，將[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所顯示的 `<handlerSettings>` 新增至即時應用程式的 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="b5027-679">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="b5027-680">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-680">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b5027-681">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-681">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-682">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-682">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="b5027-683">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-683">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="b5027-684">開啟路徑**site** > **wwwroot**的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-684">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b5027-685">選取鉛筆圖示來編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-685">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="b5027-686">新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。</span><span class="sxs-lookup"><span data-stu-id="b5027-686">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="b5027-687">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-687">Select the **Save** button.</span></span>
1. <span data-ttu-id="b5027-688">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-688">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b5027-689">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-689">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-690">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-690">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-691">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-691">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-692">開啟路徑**site** > **wwwroot**的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-692">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b5027-693">如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。</span><span class="sxs-lookup"><span data-stu-id="b5027-693">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="b5027-694">如果已提供路徑，請巡覽至記錄檔的位置。</span><span class="sxs-lookup"><span data-stu-id="b5027-694">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="b5027-695">使用檔案名稱旁的鉛筆圖示來開啟記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-695">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="b5027-696">完成疑難排解時，請停用偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-696">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="b5027-697">若要停用增強型偵錯記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="b5027-697">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="b5027-698">從 *web.config* 檔案本機移除 `<handlerSettings>` 並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-698">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="b5027-699">使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。</span><span class="sxs-lookup"><span data-stu-id="b5027-699">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="b5027-700">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-700">Save the file.</span></span>

<span data-ttu-id="b5027-701">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="b5027-701">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-702">無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-702">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-703">記錄檔大小沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-703">There's no limit on log file size.</span></span> <span data-ttu-id="b5027-704">請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-704">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="b5027-705">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-705">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-706">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-706">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="b5027-707">緩慢或懸掛應用程式（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-707">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="b5027-708">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="b5027-708">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="b5027-709">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-709">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="b5027-710">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b5027-710">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="b5027-711">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="b5027-711">Monitoring blades</span></span>

<span data-ttu-id="b5027-712">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="b5027-712">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="b5027-713">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-713">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="b5027-714">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="b5027-714">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="b5027-715">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="b5027-715">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="b5027-716">在 [開發工具]\*\*\*\* 刀鋒視窗區段中，選取 [延伸模組]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-716">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="b5027-717">[ASP.NET Core 延伸模組]\*\*\*\* 應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="b5027-717">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="b5027-718">如果未安裝這些延伸模組，請選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-718">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="b5027-719">從清單中選擇 [ASP.NET Core 延伸模組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-719">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="b5027-720">選取 [確定]\*\*\*\* 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="b5027-720">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="b5027-721">在 [新增延伸模組]\*\*\*\* 刀鋒視窗上，選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-721">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="b5027-722">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="b5027-722">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="b5027-723">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="b5027-723">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="b5027-724">在 Azure 入口網站中，選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-724">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="b5027-725">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-725">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-726">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-726">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-727">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-727">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-728">將資料夾開啟至路徑 [site]**[wwwroot]** > \*\*\*\*，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-728">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="b5027-729">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b5027-729">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="b5027-730">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="b5027-730">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="b5027-731">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-731">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="b5027-732">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-732">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="b5027-733">在 Azure 入口網站中，選取 [診斷記錄檔]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-733">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="b5027-734">選取 [應用程式記錄 (檔案系統)]\*\*\*\* 和 [詳細錯誤訊息]\*\*\*\*.的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="b5027-734">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="b5027-735">選取刀鋒視窗頂端的 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-735">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="b5027-736">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤]\*\*\*\* 的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="b5027-736">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="b5027-737">選取 [記錄資料流]\*\*\*\* 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔]\*\*\*\* 刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="b5027-737">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="b5027-738">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-738">Make a request to the app.</span></span>
1. <span data-ttu-id="b5027-739">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-739">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="b5027-740">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-740">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="b5027-741">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="b5027-741">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="b5027-742">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-742">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="b5027-743">從資訊看板的 [支援工具]\*\*\*\* 區域中，選取 [失敗要求追蹤記錄檔]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-743">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="b5027-744">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="b5027-744">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="b5027-745">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="b5027-745">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-746">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-746">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-747">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-747">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="b5027-748">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-748">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-749">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-749">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="b5027-750">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-750">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="b5027-751">應用程式事件記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-751">Application Event Log (IIS)</span></span>

<span data-ttu-id="b5027-752">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="b5027-752">Access the Application Event Log:</span></span>

1. <span data-ttu-id="b5027-753">開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-753">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="b5027-754">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="b5027-754">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="b5027-755">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="b5027-755">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="b5027-756">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-756">Search for errors associated with the failing app.</span></span> <span data-ttu-id="b5027-757">錯誤在 [來源]\*\* 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="b5027-757">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="b5027-758">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-758">Run the app at a command prompt</span></span>

<span data-ttu-id="b5027-759">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="b5027-759">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="b5027-760">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-760">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="b5027-761">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="b5027-761">Framework-dependent deployment</span></span>

<span data-ttu-id="b5027-762">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-762">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="b5027-763">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-763">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="b5027-764">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="b5027-764">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="b5027-765">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-765">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="b5027-766">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-766">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="b5027-767">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-767">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="b5027-768">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="b5027-768">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="b5027-769">自封式部署</span><span class="sxs-lookup"><span data-stu-id="b5027-769">Self-contained deployment</span></span>

<span data-ttu-id="b5027-770">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-770">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="b5027-771">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-771">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="b5027-772">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="b5027-772">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="b5027-773">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-773">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="b5027-774">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-774">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="b5027-775">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-775">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="b5027-776">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="b5027-776">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="b5027-777">ASP.NET Core 模組 stdout 記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-777">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="b5027-778">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b5027-778">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b5027-779">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-779">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="b5027-780">如果 [logs]\*\* 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-780">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="b5027-781">如需有關如何讓 MSBuild 在部署中自動建立 [logs]\*\* 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="b5027-781">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="b5027-782">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-782">Edit the *web.config* file.</span></span> <span data-ttu-id="b5027-783">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs]\*\* 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="b5027-783">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="b5027-784">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="b5027-784">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="b5027-785">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="b5027-785">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="b5027-786">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="b5027-786">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="b5027-787">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b5027-787">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="b5027-788">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-788">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="b5027-789">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-789">Make a request to the app.</span></span>
1. <span data-ttu-id="b5027-790">瀏覽至 [logs]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-790">Navigate to the *logs* folder.</span></span> <span data-ttu-id="b5027-791">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-791">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="b5027-792">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-792">Study the log for errors.</span></span>

<span data-ttu-id="b5027-793">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-793">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="b5027-794">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-794">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="b5027-795">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="b5027-795">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="b5027-796">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-796">Save the file.</span></span>

<span data-ttu-id="b5027-797">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="b5027-797">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-798">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-798">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-799">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-799">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="b5027-800">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-800">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-801">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-801">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="b5027-802">ASP.NET Core 模組的調試記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-802">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="b5027-803">將下列處理常式設定新增至應用程式*的 web.config 檔案*，以啟用 ASP.NET Core 模組的 debug 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b5027-803">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="b5027-804">確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b5027-804">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="b5027-805">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="b5027-805">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="b5027-806">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="b5027-806">Enable the Developer Exception Page</span></span>

<span data-ttu-id="b5027-807">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-807">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="b5027-808">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="b5027-808">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="b5027-809">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="b5027-809">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="b5027-810">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="b5027-810">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="b5027-811">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="b5027-811">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="b5027-812">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="b5027-812">Obtain data from an app</span></span>

<span data-ttu-id="b5027-813">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="b5027-813">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="b5027-814">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="b5027-814">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="b5027-815">緩慢或懸掛應用程式（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-815">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="b5027-816">損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-816">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="b5027-817">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="b5027-817">App crashes or encounters an exception</span></span>

<span data-ttu-id="b5027-818">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="b5027-818">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="b5027-819">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-819">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="b5027-820">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b5027-820">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="b5027-821">執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="b5027-821">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="b5027-822">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-822">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="b5027-823">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-823">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="b5027-824">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-824">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="b5027-825">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="b5027-825">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="b5027-826">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-826">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="b5027-827">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-827">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="b5027-828">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-828">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="b5027-829">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="b5027-829">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-830">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="b5027-830">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="b5027-831">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="b5027-831">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="b5027-832">當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。</span><span class="sxs-lookup"><span data-stu-id="b5027-832">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="b5027-833">分析傾印</span><span class="sxs-lookup"><span data-stu-id="b5027-833">Analyze the dump</span></span>

<span data-ttu-id="b5027-834">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="b5027-834">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="b5027-835">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="b5027-835">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="b5027-836">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="b5027-836">Clear package caches</span></span>

<span data-ttu-id="b5027-837">升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-837">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="b5027-838">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-838">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="b5027-839">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="b5027-839">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="b5027-840">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-840">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="b5027-841">從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="b5027-841">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="b5027-842">清除套件快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成，並執行命令`nuget locals all -clear`。</span><span class="sxs-lookup"><span data-stu-id="b5027-842">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="b5027-843">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="b5027-843">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="b5027-844">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="b5027-844">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="b5027-845">重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-845">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5027-846">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5027-846">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="b5027-847">Azure 文件</span><span class="sxs-lookup"><span data-stu-id="b5027-847">Azure documentation</span></span>

* [<span data-ttu-id="b5027-848">ASP.NET Core 的 Application Insights</span><span class="sxs-lookup"><span data-stu-id="b5027-848">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="b5027-849">使用 Visual Studio 在 Azure App Service 中疑難排解 web 應用程式的遠端偵錯程式一節</span><span class="sxs-lookup"><span data-stu-id="b5027-849">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="b5027-850">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="b5027-850">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="b5027-851">作法：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-851">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="b5027-852">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-852">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="b5027-853">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-853">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="b5027-854">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-854">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="b5027-855">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="b5027-855">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="b5027-856">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b5027-856">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="b5027-857">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="b5027-857">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="b5027-858">Visual Studio 文件</span><span class="sxs-lookup"><span data-stu-id="b5027-858">Visual Studio documentation</span></span>

* [<span data-ttu-id="b5027-859">Visual Studio 2017 中 Azure 上 IIS 的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5027-859">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="b5027-860">Visual Studio 2017 中遠端 IIS 電腦上的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5027-860">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="b5027-861">瞭解如何使用 Visual Studio 進行調試</span><span class="sxs-lookup"><span data-stu-id="b5027-861">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="b5027-862">Visual Studio Code 檔</span><span class="sxs-lookup"><span data-stu-id="b5027-862">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="b5027-863">使用 Visual Studio Code 偵錯</span><span class="sxs-lookup"><span data-stu-id="b5027-863">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b5027-864">本文提供有關一般應用程式啟動錯誤的資訊，以及如何在將應用程式部署至 Azure App Service 或 IIS 時診斷錯誤的指示：</span><span class="sxs-lookup"><span data-stu-id="b5027-864">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="b5027-865">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="b5027-865">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="b5027-866">說明常見的啟動 HTTP 狀態碼案例。</span><span class="sxs-lookup"><span data-stu-id="b5027-866">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="b5027-867">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-867">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="b5027-868">提供部署至 Azure App Service 之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="b5027-868">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="b5027-869">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-869">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="b5027-870">提供部署至 IIS 或在本機 IIS Express 上執行之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="b5027-870">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="b5027-871">本指南適用于 Windows Server 和 Windows 桌面部署。</span><span class="sxs-lookup"><span data-stu-id="b5027-871">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="b5027-872">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="b5027-872">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="b5027-873">說明當一致封裝在執行主要升級或變更封裝版本時，中斷應用程式時該怎麼辦。</span><span class="sxs-lookup"><span data-stu-id="b5027-873">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="b5027-874">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5027-874">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="b5027-875">列出其他疑難排解主題。</span><span class="sxs-lookup"><span data-stu-id="b5027-875">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="b5027-876">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="b5027-876">App startup errors</span></span>

<span data-ttu-id="b5027-877">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="b5027-877">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="b5027-878">使用本主題中的建議，可以診斷本機上的偵錯工具發生的*502.5 進程失敗*。</span><span class="sxs-lookup"><span data-stu-id="b5027-878">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="b5027-879">403.14 禁止</span><span class="sxs-lookup"><span data-stu-id="b5027-879">403.14 Forbidden</span></span>

<span data-ttu-id="b5027-880">應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-880">The app fails to start.</span></span> <span data-ttu-id="b5027-881">會記錄下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="b5027-881">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="b5027-882">此錯誤通常是因為主控系統上的部署中斷所造成，其中包括下列任何一種情況：</span><span class="sxs-lookup"><span data-stu-id="b5027-882">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="b5027-883">應用程式會部署到裝載系統上錯誤的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-883">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="b5027-884">部署進程無法將應用程式的所有檔案和資料夾移到主控系統上的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-884">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="b5027-885">部署中遺漏*web.config*檔案，或*web.config*檔案內容的格式不正確。</span><span class="sxs-lookup"><span data-stu-id="b5027-885">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="b5027-886">執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b5027-886">Perform the following steps:</span></span>

1. <span data-ttu-id="b5027-887">刪除主控系統上部署資料夾中的所有檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-887">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="b5027-888">使用一般部署方法（例如 Visual Studio、PowerShell 或手動部署），將應用程式的 [*發佈*] 資料夾的內容重新部署至主機系統：</span><span class="sxs-lookup"><span data-stu-id="b5027-888">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="b5027-889">請確認*web.config*檔案存在於部署中，而且其內容正確。</span><span class="sxs-lookup"><span data-stu-id="b5027-889">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="b5027-890">在 Azure App Service 上裝載時，請確認應用程式已部署至`D:\home\site\wwwroot`資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-890">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="b5027-891">當應用程式由 IIS 裝載時，請確認應用程式已部署到 iis**管理員**的 [**基本設定**] 中所顯示的 iis**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="b5027-891">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="b5027-892">藉由比較主控系統上的部署與專案 [*發行*] 資料夾的內容，確認已部署所有應用程式的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-892">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="b5027-893">如需已發佈之 ASP.NET Core 應用程式佈建的詳細資訊， <xref:host-and-deploy/directory-structure>請參閱。</span><span class="sxs-lookup"><span data-stu-id="b5027-893">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="b5027-894">如需*web.config*檔案的詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="b5027-894">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="b5027-895">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="b5027-895">500 Internal Server Error</span></span>

<span data-ttu-id="b5027-896">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-896">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="b5027-897">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="b5027-897">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="b5027-898">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」\*\* 的形式出現。</span><span class="sxs-lookup"><span data-stu-id="b5027-898">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="b5027-899">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-899">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="b5027-900">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="b5027-900">From the server's perspective, that's correct.</span></span> <span data-ttu-id="b5027-901">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="b5027-901">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="b5027-902">請在伺服器上於命令提示字元中執行應用程式或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="b5027-902">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="b5027-903">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="b5027-903">502.5 Process Failure</span></span>

<span data-ttu-id="b5027-904">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-904">The worker process fails.</span></span> <span data-ttu-id="b5027-905">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-905">The app doesn't start.</span></span>

<span data-ttu-id="b5027-906">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-906">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="b5027-907">通常從應用程式事件記錄檔和 ASP.NET Core 模組 stdout 記錄檔中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-907">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="b5027-908">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="b5027-908">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="b5027-909">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="b5027-909">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="b5027-910">「共用的架構」\*\* 是一組安裝於電腦上且由 `Microsoft.AspNetCore.App` 之類的中繼套件所參考的組件 (*.dll* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="b5027-910">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="b5027-911">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="b5027-911">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="b5027-912">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="b5027-912">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="b5027-913">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗]\*\* 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="b5027-913">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="b5027-914">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="b5027-914">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="b5027-915">應用程式無法啟動，因為無法載入應用程式的組件 (*.dll*)。</span><span class="sxs-lookup"><span data-stu-id="b5027-915">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="b5027-916">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-916">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="b5027-917">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="b5027-917">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="b5027-918">在 IIS 管理員的 [應用程式集區]\*\*\*\* 中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="b5027-918">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="b5027-919">在 [動作]\*\*\*\* 面板中，選取 [編輯應用程式集區]\*\*\*\* 下的 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-919">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="b5027-920">設定 [啟用 32 位元應用程式]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="b5027-920">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="b5027-921">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="b5027-921">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="b5027-922">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="b5027-922">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="b5027-923">確認專案檔中的`<Platform>` MSBuild 屬性和應用程式的已發佈位之間沒有衝突。</span><span class="sxs-lookup"><span data-stu-id="b5027-923">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="b5027-924">連線重設</span><span class="sxs-lookup"><span data-stu-id="b5027-924">Connection reset</span></span>

<span data-ttu-id="b5027-925">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-925">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="b5027-926">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-926">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="b5027-927">這類錯誤會在用戶端上顯示為「連線重設」\*\* 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-927">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="b5027-928">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="b5027-928">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="b5027-929">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="b5027-929">Default startup limits</span></span>

<span data-ttu-id="b5027-930">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是以120秒的預設*startupTimeLimit*來設定。</span><span class="sxs-lookup"><span data-stu-id="b5027-930">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="b5027-931">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="b5027-931">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="b5027-932">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="b5027-932">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="b5027-933">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-933">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="b5027-934">應用程式事件記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-934">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="b5027-935">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題]\*\*\*\* 刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="b5027-935">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="b5027-936">在 Azure 入口網站的 [應用程式服務]\*\*\*\* 中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-936">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="b5027-937">選取 [診斷並解決問題]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-937">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="b5027-938">選取 [診斷工具]\*\*\*\* 標題。</span><span class="sxs-lookup"><span data-stu-id="b5027-938">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="b5027-939">在 [支援工具]\*\*\*\* 下，選取 [應用程式事件]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-939">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="b5027-940">檢查 [來源]\*\*\*\* 資料行中 *IIS AspNetCoreModule* 或 *IIS AspNetCoreModule V2* 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-940">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="b5027-941">除了使用 [診斷並解決問題]\*\*\*\* 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="b5027-941">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="b5027-942">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-942">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b5027-943">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-943">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-944">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-944">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-945">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-945">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-946">開啟 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-946">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="b5027-947">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b5027-947">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="b5027-948">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-948">Examine the log.</span></span> <span data-ttu-id="b5027-949">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="b5027-949">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="b5027-950">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-950">Run the app in the Kudu console</span></span>

<span data-ttu-id="b5027-951">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="b5027-951">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="b5027-952">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="b5027-952">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="b5027-953">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-953">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b5027-954">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-954">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-955">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-955">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-956">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-956">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="b5027-957">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-957">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="b5027-958">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="b5027-958">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="b5027-959">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="b5027-959">Run the app:</span></span>
   * <span data-ttu-id="b5027-960">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-960">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="b5027-961">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-961">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="b5027-962">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-962">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="b5027-963">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="b5027-963">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="b5027-964">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="b5027-964">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="b5027-965">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="b5027-965">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="b5027-966">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b5027-966">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="b5027-967">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-967">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="b5027-968">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-968">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="b5027-969">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="b5027-969">**Current release**</span></span>

* <span data-ttu-id="b5027-970">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-970">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="b5027-971">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b5027-971">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="b5027-972">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-972">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="b5027-973">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="b5027-973">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="b5027-974">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-974">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="b5027-975">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="b5027-975">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="b5027-976">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="b5027-976">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="b5027-977">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="b5027-977">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="b5027-978">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b5027-978">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="b5027-979">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="b5027-979">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="b5027-980">ASP.NET Core 模組 stdout 記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-980">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="b5027-981">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="b5027-981">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="b5027-982">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b5027-982">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b5027-983">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-983">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="b5027-984">在 [選取問題類別]\*\*\*\* 底下，選取 [Web 應用程式停止運作]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-984">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="b5027-985">在 [建議的解決方案]**[啟用 Stdout 記錄檔重新導向]** > \*\*\*\* 底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-985">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="b5027-986">在 Kudu [診斷主控台]\*\*\*\* 中，將資料夾開啟至路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-986">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b5027-987">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-987">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="b5027-988">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b5027-988">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="b5027-989">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="b5027-989">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="b5027-990">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-990">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="b5027-991">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-991">Make a request to the app.</span></span>
1. <span data-ttu-id="b5027-992">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="b5027-992">Return to the Azure portal.</span></span> <span data-ttu-id="b5027-993">選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-993">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="b5027-994">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-994">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-995">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-995">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-996">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-996">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-997">選取 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-997">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="b5027-998">檢查 [修改時間]\*\*\*\* 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-998">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="b5027-999">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-999">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="b5027-1000">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-1000">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="b5027-1001">在 Kudu [診斷主控台]\*\*\*\* 中，返回路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\* 以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1001">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="b5027-1002">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1002">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="b5027-1003">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="b5027-1003">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="b5027-1004">選取 [儲存]\*\*\*\* 以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1004">Select **Save** to save the file.</span></span>

<span data-ttu-id="b5027-1005">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="b5027-1005">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-1006">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1006">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-1007">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-1007">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="b5027-1008">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-1008">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="b5027-1009">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-1009">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-1010">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1010">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="b5027-1011">緩慢或懸掛應用程式（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="b5027-1011">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="b5027-1012">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="b5027-1012">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="b5027-1013">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-1013">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="b5027-1014">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b5027-1014">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="b5027-1015">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="b5027-1015">Monitoring blades</span></span>

<span data-ttu-id="b5027-1016">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1016">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="b5027-1017">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-1017">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="b5027-1018">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="b5027-1018">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="b5027-1019">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="b5027-1019">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="b5027-1020">在 [開發工具]\*\*\*\* 刀鋒視窗區段中，選取 [延伸模組]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1020">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="b5027-1021">[ASP.NET Core 延伸模組]\*\*\*\* 應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="b5027-1021">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="b5027-1022">如果未安裝這些延伸模組，請選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-1022">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="b5027-1023">從清單中選擇 [ASP.NET Core 延伸模組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-1023">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="b5027-1024">選取 [確定]\*\*\*\* 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="b5027-1024">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="b5027-1025">在 [新增延伸模組]\*\*\*\* 刀鋒視窗上，選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-1025">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="b5027-1026">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="b5027-1026">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="b5027-1027">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="b5027-1027">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="b5027-1028">在 Azure 入口網站中，選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1028">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="b5027-1029">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-1029">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b5027-1030">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="b5027-1030">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b5027-1031">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-1031">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b5027-1032">將資料夾開啟至路徑 [site]**[wwwroot]** > \*\*\*\*，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1032">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="b5027-1033">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="b5027-1033">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="b5027-1034">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="b5027-1034">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="b5027-1035">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1035">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="b5027-1036">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-1036">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="b5027-1037">在 Azure 入口網站中，選取 [診斷記錄檔]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1037">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="b5027-1038">選取 [應用程式記錄 (檔案系統)]\*\*\*\* 和 [詳細錯誤訊息]\*\*\*\*.的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="b5027-1038">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="b5027-1039">選取刀鋒視窗頂端的 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5027-1039">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="b5027-1040">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤]\*\*\*\* 的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="b5027-1040">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="b5027-1041">選取 [記錄資料流]\*\*\*\* 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔]\*\*\*\* 刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1041">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="b5027-1042">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-1042">Make a request to the app.</span></span>
1. <span data-ttu-id="b5027-1043">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-1043">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="b5027-1044">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="b5027-1044">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="b5027-1045">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="b5027-1045">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="b5027-1046">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1046">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="b5027-1047">從資訊看板的 [支援工具]\*\*\*\* 區域中，選取 [失敗要求追蹤記錄檔]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b5027-1047">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="b5027-1048">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1048">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="b5027-1049">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1049">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-1050">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1050">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-1051">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-1051">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="b5027-1052">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-1052">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-1053">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1053">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="b5027-1054">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-1054">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="b5027-1055">應用程式事件記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-1055">Application Event Log (IIS)</span></span>

<span data-ttu-id="b5027-1056">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="b5027-1056">Access the Application Event Log:</span></span>

1. <span data-ttu-id="b5027-1057">開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-1057">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="b5027-1058">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="b5027-1058">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="b5027-1059">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="b5027-1059">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="b5027-1060">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-1060">Search for errors associated with the failing app.</span></span> <span data-ttu-id="b5027-1061">錯誤在 [來源]\*\* 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="b5027-1061">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="b5027-1062">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-1062">Run the app at a command prompt</span></span>

<span data-ttu-id="b5027-1063">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="b5027-1063">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="b5027-1064">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-1064">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="b5027-1065">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="b5027-1065">Framework-dependent deployment</span></span>

<span data-ttu-id="b5027-1066">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-1066">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="b5027-1067">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-1067">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="b5027-1068">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="b5027-1068">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="b5027-1069">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1069">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="b5027-1070">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-1070">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="b5027-1071">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-1071">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="b5027-1072">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="b5027-1072">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="b5027-1073">自封式部署</span><span class="sxs-lookup"><span data-stu-id="b5027-1073">Self-contained deployment</span></span>

<span data-ttu-id="b5027-1074">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="b5027-1074">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="b5027-1075">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-1075">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="b5027-1076">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="b5027-1076">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="b5027-1077">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1077">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="b5027-1078">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-1078">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="b5027-1079">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-1079">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="b5027-1080">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="b5027-1080">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="b5027-1081">ASP.NET Core 模組 stdout 記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-1081">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="b5027-1082">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="b5027-1082">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b5027-1083">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-1083">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="b5027-1084">如果 [logs]\*\* 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-1084">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="b5027-1085">如需有關如何讓 MSBuild 在部署中自動建立 [logs]\*\* 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="b5027-1085">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="b5027-1086">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1086">Edit the *web.config* file.</span></span> <span data-ttu-id="b5027-1087">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs]\*\* 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1087">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="b5027-1088">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="b5027-1088">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="b5027-1089">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="b5027-1089">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="b5027-1090">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="b5027-1090">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="b5027-1091">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b5027-1091">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="b5027-1092">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1092">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="b5027-1093">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="b5027-1093">Make a request to the app.</span></span>
1. <span data-ttu-id="b5027-1094">瀏覽至 [logs]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-1094">Navigate to the *logs* folder.</span></span> <span data-ttu-id="b5027-1095">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b5027-1095">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="b5027-1096">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5027-1096">Study the log for errors.</span></span>

<span data-ttu-id="b5027-1097">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="b5027-1097">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="b5027-1098">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1098">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="b5027-1099">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="b5027-1099">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="b5027-1100">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1100">Save the file.</span></span>

<span data-ttu-id="b5027-1101">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="b5027-1101">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-1102">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1102">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b5027-1103">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="b5027-1103">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="b5027-1104">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="b5027-1104">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5027-1105">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1105">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="b5027-1106">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="b5027-1106">Enable the Developer Exception Page</span></span>

<span data-ttu-id="b5027-1107">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-1107">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="b5027-1108">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1108">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="b5027-1109">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="b5027-1109">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="b5027-1110">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="b5027-1110">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="b5027-1111">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1111">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="b5027-1112">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="b5027-1112">Obtain data from an app</span></span>

<span data-ttu-id="b5027-1113">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="b5027-1113">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="b5027-1114">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="b5027-1114">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="b5027-1115">緩慢或懸掛應用程式（IIS）</span><span class="sxs-lookup"><span data-stu-id="b5027-1115">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="b5027-1116">損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="b5027-1116">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="b5027-1117">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="b5027-1117">App crashes or encounters an exception</span></span>

<span data-ttu-id="b5027-1118">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="b5027-1118">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="b5027-1119">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1119">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="b5027-1120">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b5027-1120">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="b5027-1121">執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="b5027-1121">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="b5027-1122">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-1122">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="b5027-1123">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-1123">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="b5027-1124">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-1124">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="b5027-1125">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="b5027-1125">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="b5027-1126">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-1126">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="b5027-1127">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="b5027-1127">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="b5027-1128">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-1128">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="b5027-1129">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="b5027-1129">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="b5027-1130">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1130">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="b5027-1131">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="b5027-1131">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="b5027-1132">當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。</span><span class="sxs-lookup"><span data-stu-id="b5027-1132">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="b5027-1133">分析傾印</span><span class="sxs-lookup"><span data-stu-id="b5027-1133">Analyze the dump</span></span>

<span data-ttu-id="b5027-1134">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="b5027-1134">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="b5027-1135">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="b5027-1135">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="b5027-1136">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="b5027-1136">Clear package caches</span></span>

<span data-ttu-id="b5027-1137">升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="b5027-1137">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="b5027-1138">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5027-1138">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="b5027-1139">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="b5027-1139">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="b5027-1140">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5027-1140">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="b5027-1141">從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="b5027-1141">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="b5027-1142">清除套件快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成，並執行命令`nuget locals all -clear`。</span><span class="sxs-lookup"><span data-stu-id="b5027-1142">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="b5027-1143">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="b5027-1143">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="b5027-1144">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1144">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="b5027-1145">重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="b5027-1145">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5027-1146">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5027-1146">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="b5027-1147">Azure 文件</span><span class="sxs-lookup"><span data-stu-id="b5027-1147">Azure documentation</span></span>

* [<span data-ttu-id="b5027-1148">ASP.NET Core 的 Application Insights</span><span class="sxs-lookup"><span data-stu-id="b5027-1148">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="b5027-1149">使用 Visual Studio 在 Azure App Service 中疑難排解 web 應用程式的遠端偵錯程式一節</span><span class="sxs-lookup"><span data-stu-id="b5027-1149">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="b5027-1150">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="b5027-1150">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="b5027-1151">作法：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-1151">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="b5027-1152">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b5027-1152">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="b5027-1153">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-1153">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="b5027-1154">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5027-1154">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="b5027-1155">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="b5027-1155">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="b5027-1156">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b5027-1156">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="b5027-1157">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="b5027-1157">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="b5027-1158">Visual Studio 文件</span><span class="sxs-lookup"><span data-stu-id="b5027-1158">Visual Studio documentation</span></span>

* [<span data-ttu-id="b5027-1159">Visual Studio 2017 中 Azure 上 IIS 的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5027-1159">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="b5027-1160">Visual Studio 2017 中遠端 IIS 電腦上的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5027-1160">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="b5027-1161">瞭解如何使用 Visual Studio 進行調試</span><span class="sxs-lookup"><span data-stu-id="b5027-1161">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="b5027-1162">Visual Studio Code 檔</span><span class="sxs-lookup"><span data-stu-id="b5027-1162">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="b5027-1163">使用 Visual Studio Code 偵錯</span><span class="sxs-lookup"><span data-stu-id="b5027-1163">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)

::: moniker-end
