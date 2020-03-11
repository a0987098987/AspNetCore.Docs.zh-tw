---
title: 疑難排解 Azure App Service 和 IIS 上的 ASP.NET Core
author: rick-anderson
description: 瞭解如何診斷 ASP.NET Core 應用程式的 Azure App Service 和 Internet Information Services （IIS）部署問題。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 671f68da2ea261cb8ae32a9d5ef875217859054d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655327"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="7151d-103">疑難排解 Azure App Service 和 IIS 上的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7151d-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="7151d-104">作者：[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="7151d-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7151d-105">本文提供有關一般應用程式啟動錯誤的資訊，以及如何在將應用程式部署至 Azure App Service 或 IIS 時診斷錯誤的指示：</span><span class="sxs-lookup"><span data-stu-id="7151d-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="7151d-106">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="7151d-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="7151d-107">說明常見的啟動 HTTP 狀態碼案例。</span><span class="sxs-lookup"><span data-stu-id="7151d-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="7151d-108">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="7151d-109">提供部署至 Azure App Service 之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="7151d-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="7151d-110">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="7151d-111">提供部署至 IIS 或在本機 IIS Express 上執行之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="7151d-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="7151d-112">本指南適用于 Windows Server 和 Windows 桌面部署。</span><span class="sxs-lookup"><span data-stu-id="7151d-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="7151d-113">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="7151d-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="7151d-114">說明當一致封裝在執行主要升級或變更封裝版本時，中斷應用程式時該怎麼辦。</span><span class="sxs-lookup"><span data-stu-id="7151d-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="7151d-115">其他資源</span><span class="sxs-lookup"><span data-stu-id="7151d-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="7151d-116">列出其他疑難排解主題。</span><span class="sxs-lookup"><span data-stu-id="7151d-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="7151d-117">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="7151d-117">App startup errors</span></span>

<span data-ttu-id="7151d-118">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="7151d-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="7151d-119">使用本主題中的建議，可以診斷在本機進行偵錯工具時所發生的*502.5-進程失敗*或*500.30 啟動失敗*。</span><span class="sxs-lookup"><span data-stu-id="7151d-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="7151d-120">403.14 禁止</span><span class="sxs-lookup"><span data-stu-id="7151d-120">403.14 Forbidden</span></span>

<span data-ttu-id="7151d-121">應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-121">The app fails to start.</span></span> <span data-ttu-id="7151d-122">會記錄下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="7151d-122">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="7151d-123">此錯誤通常是因為主控系統上的部署中斷所造成，其中包括下列任何一種情況：</span><span class="sxs-lookup"><span data-stu-id="7151d-123">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="7151d-124">應用程式會部署到裝載系統上錯誤的資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-124">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="7151d-125">部署進程無法將應用程式的所有檔案和資料夾移到主控系統上的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-125">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="7151d-126">部署中遺漏*web.config*檔案，或*web.config*檔案內容的格式不正確。</span><span class="sxs-lookup"><span data-stu-id="7151d-126">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="7151d-127">執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="7151d-127">Perform the following steps:</span></span>

1. <span data-ttu-id="7151d-128">刪除主控系統上部署資料夾中的所有檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-128">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="7151d-129">使用一般部署方法（例如 Visual Studio、PowerShell 或手動部署），將應用程式的 [*發佈*] 資料夾的內容重新部署至主機系統：</span><span class="sxs-lookup"><span data-stu-id="7151d-129">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="7151d-130">請確認*web.config*檔案存在於部署中，而且其內容正確。</span><span class="sxs-lookup"><span data-stu-id="7151d-130">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="7151d-131">在 Azure App Service 上裝載時，請確認應用程式已部署至 `D:\home\site\wwwroot` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-131">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="7151d-132">當應用程式由 IIS 裝載時，請確認應用程式已部署到 iis**管理員**的 [**基本設定**] 中所顯示的 iis**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="7151d-132">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="7151d-133">藉由比較主控系統上的部署與專案 [*發行*] 資料夾的內容，確認已部署所有應用程式的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-133">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="7151d-134">如需已發佈 ASP.NET Core 應用程式佈建的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="7151d-134">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="7151d-135">如需*web.config*檔案的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="7151d-135">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="7151d-136">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="7151d-136">500 Internal Server Error</span></span>

<span data-ttu-id="7151d-137">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-137">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="7151d-138">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="7151d-138">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="7151d-139">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」的形式出現。</span><span class="sxs-lookup"><span data-stu-id="7151d-139">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="7151d-140">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-140">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="7151d-141">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="7151d-141">From the server's perspective, that's correct.</span></span> <span data-ttu-id="7151d-142">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="7151d-142">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="7151d-143">在伺服器上的命令提示字元中執行應用程式，或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="7151d-143">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="7151d-144">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="7151d-144">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="7151d-145">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-145">The worker process fails.</span></span> <span data-ttu-id="7151d-146">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-146">The app doesn't start.</span></span>

<span data-ttu-id="7151d-147">載入[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)元件時發生未知的錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-147">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="7151d-148">請採取下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="7151d-148">Take one of the following actions:</span></span>

* <span data-ttu-id="7151d-149">連絡 [Microsoft 支援服務](https://support.microsoft.com/oas/default.aspx?prid=15832) (依序選取 [開發人員工具] 和 [ASP.NET Core])。</span><span class="sxs-lookup"><span data-stu-id="7151d-149">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="7151d-150">在 Stack Overflow 上詢問問題。</span><span class="sxs-lookup"><span data-stu-id="7151d-150">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="7151d-151">在我們的 [GitHub 存放庫](https://github.com/dotnet/AspNetCore)提出問題。</span><span class="sxs-lookup"><span data-stu-id="7151d-151">File an issue on our [GitHub repository](https://github.com/dotnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="7151d-152">500.30 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="7151d-152">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="7151d-153">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-153">The worker process fails.</span></span> <span data-ttu-id="7151d-154">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-154">The app doesn't start.</span></span>

<span data-ttu-id="7151d-155">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動 .NET Core CLR 同進程，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-155">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="7151d-156">通常會從應用程式事件記錄檔中的專案和 ASP.NET Core 模組 stdout 記錄檔中判斷進程啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-156">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="7151d-157">常見的失敗狀況：</span><span class="sxs-lookup"><span data-stu-id="7151d-157">Common failure conditions:</span></span>

* <span data-ttu-id="7151d-158">應用程式設定錯誤，因為以不存在的 ASP.NET Core 共用架構版本為目標。</span><span class="sxs-lookup"><span data-stu-id="7151d-158">The app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="7151d-159">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="7151d-159">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>
* <span data-ttu-id="7151d-160">使用 Azure Key Vault，缺少 Key Vault 的許可權。</span><span class="sxs-lookup"><span data-stu-id="7151d-160">Using Azure Key Vault, lack of permissions to the Key Vault.</span></span> <span data-ttu-id="7151d-161">檢查目標 Key Vault 中的存取原則，以確定已授與正確的許可權。</span><span class="sxs-lookup"><span data-stu-id="7151d-161">Check the access policies in the targeted Key Vault to ensure that the correct permissions are granted.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="7151d-162">500.31 ANCM 找不到原生相依性</span><span class="sxs-lookup"><span data-stu-id="7151d-162">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="7151d-163">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-163">The worker process fails.</span></span> <span data-ttu-id="7151d-164">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-164">The app doesn't start.</span></span>

<span data-ttu-id="7151d-165">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試在同進程中啟動 .net Core 執行時間，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-165">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="7151d-166">此啟動失敗的最常見原因是當 `Microsoft.NETCore.App` 或 `Microsoft.AspNetCore.App` 執行階段未安裝時。</span><span class="sxs-lookup"><span data-stu-id="7151d-166">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="7151d-167">如果應用程式部署至目標 ASP.NET Core 3.0，但電腦上無該版本，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-167">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="7151d-168">範例錯誤訊息如下：</span><span class="sxs-lookup"><span data-stu-id="7151d-168">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="7151d-169">錯誤訊息會列出所有已安裝 .NET Core 版本和應用程式所要求的版本。</span><span class="sxs-lookup"><span data-stu-id="7151d-169">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="7151d-170">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="7151d-170">To fix this error, either:</span></span>

* <span data-ttu-id="7151d-171">在電腦上安裝適當的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="7151d-171">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="7151d-172">將應用程式的目標 .NET Core 版本變更為電腦上版本。</span><span class="sxs-lookup"><span data-stu-id="7151d-172">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="7151d-173">將應用程式發佈為[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="7151d-173">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="7151d-174">在開發過程中執行時 (`ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`)，特定的錯誤會寫入至 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="7151d-174">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="7151d-175">在應用程式事件記錄檔中也可以找到進程啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-175">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="7151d-176">500.32 ANCM 無法載入 dll</span><span class="sxs-lookup"><span data-stu-id="7151d-176">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="7151d-177">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-177">The worker process fails.</span></span> <span data-ttu-id="7151d-178">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-178">The app doesn't start.</span></span>

<span data-ttu-id="7151d-179">此錯誤最常見原因是針對不相容的處理器架構發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-179">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="7151d-180">如果背景工作處理序執行為 32 位元應用程式，而此應用程式已發佈至目標 64 位元，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-180">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="7151d-181">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="7151d-181">To fix this error, either:</span></span>

* <span data-ttu-id="7151d-182">針對相同的處理器架構，將應用程式重新發佈為背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="7151d-182">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="7151d-183">將應用程式發佈為[架構相依部署](/dotnet/core/deploying/#framework-dependent-executables-fde)。</span><span class="sxs-lookup"><span data-stu-id="7151d-183">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="7151d-184">500.33 ANCM 要求處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="7151d-184">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="7151d-185">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-185">The worker process fails.</span></span> <span data-ttu-id="7151d-186">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-186">The app doesn't start.</span></span>

<span data-ttu-id="7151d-187">應用程式未參考 `Microsoft.AspNetCore.App` 架構。</span><span class="sxs-lookup"><span data-stu-id="7151d-187">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="7151d-188">只有以 `Microsoft.AspNetCore.App` 架構為目標的應用程式可以由[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主控。</span><span class="sxs-lookup"><span data-stu-id="7151d-188">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="7151d-189">若要修正這個錯誤，請確認應用程式以 `Microsoft.AspNetCore.App` 架構為目標。</span><span class="sxs-lookup"><span data-stu-id="7151d-189">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="7151d-190">檢查 `.runtimeconfig.json` 以驗證應用程式是否以該架構為目標。</span><span class="sxs-lookup"><span data-stu-id="7151d-190">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="7151d-191">500.34 ANCM 不支援混合式裝載模型</span><span class="sxs-lookup"><span data-stu-id="7151d-191">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="7151d-192">背景工作處理序無法在相同的程序中執行同處理序應用程式和跨處理序應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-192">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="7151d-193">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-193">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="7151d-194">500.35 ANCM 同一程序中有多個同處理序應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-194">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="7151d-195">工作者進程無法在同一個進程中執行多個同進程應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-195">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="7151d-196">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-196">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="7151d-197">500.36 ANCM 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="7151d-197">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="7151d-198">跨處理序要求處理常式 *aspnetcorev2_outofprocess.dll* 不在 *aspnetcorev2.dll* 檔案旁邊。</span><span class="sxs-lookup"><span data-stu-id="7151d-198">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="7151d-199">這表示[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)安裝損毀。</span><span class="sxs-lookup"><span data-stu-id="7151d-199">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="7151d-200">若要修正這個錯誤，請修復 [.NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (適用於 IIS) 或 Visual Studio (適用於 IIS Express) 安裝。</span><span class="sxs-lookup"><span data-stu-id="7151d-200">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="7151d-201">500.37 ANCM 無法在啟動時間限制內啟動</span><span class="sxs-lookup"><span data-stu-id="7151d-201">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="7151d-202">ANCM 無法在提供的啟動時間限制內啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-202">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="7151d-203">根據預設，逾時值為 120 秒。</span><span class="sxs-lookup"><span data-stu-id="7151d-203">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="7151d-204">在同一部電腦上啟動大量的應用程式時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-204">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="7151d-205">檢查伺服器在啟動期間是否出現 CPU/記憶體的使用量尖峰。</span><span class="sxs-lookup"><span data-stu-id="7151d-205">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="7151d-206">多個應用程式的啟動程序可能需要交錯進行。</span><span class="sxs-lookup"><span data-stu-id="7151d-206">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="7151d-207">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="7151d-207">502.5 Process Failure</span></span>

<span data-ttu-id="7151d-208">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-208">The worker process fails.</span></span> <span data-ttu-id="7151d-209">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-209">The app doesn't start.</span></span>

<span data-ttu-id="7151d-210">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-210">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="7151d-211">通常會從應用程式事件記錄檔中的專案和 ASP.NET Core 模組 stdout 記錄檔中判斷進程啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-211">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="7151d-212">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="7151d-212">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="7151d-213">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="7151d-213">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="7151d-214">「共用的架構」是一組安裝於電腦上且由  *之類的中繼套件所參考的組件 (* .dll`Microsoft.AspNetCore.App` 檔案)。</span><span class="sxs-lookup"><span data-stu-id="7151d-214">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="7151d-215">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="7151d-215">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="7151d-216">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="7151d-216">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="7151d-217">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗] 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="7151d-217">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="7151d-218">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="7151d-218">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="7151d-219">應用程式無法啟動，因為無法載入應用程式的組件 ( *.dll*)。</span><span class="sxs-lookup"><span data-stu-id="7151d-219">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="7151d-220">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-220">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="7151d-221">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="7151d-221">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="7151d-222">在 IIS 管理員的 [應用程式集區] 中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="7151d-222">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="7151d-223">在 [動作] 面板中，選取 [編輯應用程式集區] 下的 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="7151d-223">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="7151d-224">設定 [啟用 32 位元應用程式]：</span><span class="sxs-lookup"><span data-stu-id="7151d-224">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="7151d-225">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="7151d-225">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="7151d-226">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="7151d-226">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="7151d-227">確認專案檔中的 `<Platform>` MSBuild 屬性和應用程式的已發佈位之間沒有衝突。</span><span class="sxs-lookup"><span data-stu-id="7151d-227">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="7151d-228">連線重設</span><span class="sxs-lookup"><span data-stu-id="7151d-228">Connection reset</span></span>

<span data-ttu-id="7151d-229">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="7151d-229">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="7151d-230">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-230">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="7151d-231">這類錯誤會在用戶端上顯示為「連線重設」錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-231">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="7151d-232">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="7151d-232">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="7151d-233">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="7151d-233">Default startup limits</span></span>

<span data-ttu-id="7151d-234">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是以120秒的預設*startupTimeLimit*來設定。</span><span class="sxs-lookup"><span data-stu-id="7151d-234">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="7151d-235">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-235">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="7151d-236">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="7151d-236">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="7151d-237">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-237">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="7151d-238">應用程式事件記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-238">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="7151d-239">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題] 刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="7151d-239">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="7151d-240">在 Azure 入口網站的 [應用程式服務] 中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-240">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="7151d-241">選取 [診斷並解決問題]。</span><span class="sxs-lookup"><span data-stu-id="7151d-241">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="7151d-242">選取 [診斷工具] 標題。</span><span class="sxs-lookup"><span data-stu-id="7151d-242">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="7151d-243">在 [支援工具] 下，選取 [應用程式事件] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-243">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="7151d-244">檢查 [來源] 資料行中 *IIS AspNetCoreModule* 或 **IIS AspNetCoreModule V2** 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-244">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="7151d-245">除了使用 [診斷並解決問題] 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="7151d-245">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="7151d-246">在 [開發工具] 區域中，開啟 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="7151d-246">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="7151d-247">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-247">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-248">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-248">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-249">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-249">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-250">開啟 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-250">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="7151d-251">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="7151d-251">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="7151d-252">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-252">Examine the log.</span></span> <span data-ttu-id="7151d-253">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="7151d-253">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="7151d-254">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-254">Run the app in the Kudu console</span></span>

<span data-ttu-id="7151d-255">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="7151d-255">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="7151d-256">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="7151d-256">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="7151d-257">在 [開發工具] 區域中，開啟 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="7151d-257">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="7151d-258">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-258">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-259">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-259">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-260">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-260">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="7151d-261">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-261">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="7151d-262">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="7151d-262">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="7151d-263">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="7151d-263">Run the app:</span></span>
   * <span data-ttu-id="7151d-264">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-264">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="7151d-265">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-265">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="7151d-266">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-266">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="7151d-267">**在預覽版本上執行的 Framework 相依部署**</span><span class="sxs-lookup"><span data-stu-id="7151d-267">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="7151d-268">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="7151d-268">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="7151d-269">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="7151d-269">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="7151d-270">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="7151d-270">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="7151d-271">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-271">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="7151d-272">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-272">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="7151d-273">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="7151d-273">**Current release**</span></span>

* <span data-ttu-id="7151d-274">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-274">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="7151d-275">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="7151d-275">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="7151d-276">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-276">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="7151d-277">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="7151d-277">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="7151d-278">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-278">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="7151d-279">**在預覽版本上執行的 Framework 相依部署**</span><span class="sxs-lookup"><span data-stu-id="7151d-279">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="7151d-280">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="7151d-280">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="7151d-281">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="7151d-281">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="7151d-282">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="7151d-282">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="7151d-283">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-283">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="7151d-284">ASP.NET Core 模組 stdout 記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-284">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="7151d-285">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="7151d-285">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="7151d-286">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="7151d-286">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="7151d-287">在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-287">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="7151d-288">在 [選取問題類別] 底下，選取 [Web 應用程式停止運作] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-288">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="7151d-289">在 **建議的解決方案** 底下 >**啟用 Stdout 記錄**檔重新導向，選取  按鈕以**開啟 Kudu 主控台 以編輯 web.config**。</span><span class="sxs-lookup"><span data-stu-id="7151d-289">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="7151d-290">在 Kudu [診斷主控台] 中，將資料夾開啟至路徑 [site] > [wwwroot]。</span><span class="sxs-lookup"><span data-stu-id="7151d-290">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="7151d-291">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-291">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="7151d-292">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="7151d-292">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="7151d-293">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="7151d-293">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="7151d-294">選取 [儲存] 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-294">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="7151d-295">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-295">Make a request to the app.</span></span>
1. <span data-ttu-id="7151d-296">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="7151d-296">Return to the Azure portal.</span></span> <span data-ttu-id="7151d-297">選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-297">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="7151d-298">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-298">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-299">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-299">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-300">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-300">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-301">選取 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-301">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="7151d-302">檢查 [修改時間] 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-302">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="7151d-303">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-303">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="7151d-304">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-304">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="7151d-305">在 Kudu [診斷主控台] 中，返回路徑 [site] > [wwwroot] 以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-305">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="7151d-306">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-306">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="7151d-307">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="7151d-307">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="7151d-308">選取 [儲存] 以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-308">Select **Save** to save the file.</span></span>

<span data-ttu-id="7151d-309">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="7151d-309">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-310">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-310">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-311">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-311">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="7151d-312">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-312">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="7151d-313">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-313">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-314">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-314">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="7151d-315">ASP.NET Core 模組的 debug 記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-315">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="7151d-316">ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-316">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="7151d-317">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="7151d-317">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="7151d-318">若要啟用增強型診斷記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="7151d-318">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="7151d-319">遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-319">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="7151d-320">重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-320">Redeploy the app.</span></span>
   * <span data-ttu-id="7151d-321">使用 Kudu 主控台，將`<handlerSettings>`增強型診斷記錄[中所顯示的 ](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) 新增至即時應用程式的 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="7151d-321">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="7151d-322">在 [開發工具] 區域中，開啟 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="7151d-322">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="7151d-323">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-323">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-324">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-324">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="7151d-325">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-325">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="7151d-326">將資料夾開啟至路徑 [site] > [wwwroot]。</span><span class="sxs-lookup"><span data-stu-id="7151d-326">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="7151d-327">選取鉛筆圖示來編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-327">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="7151d-328">新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。</span><span class="sxs-lookup"><span data-stu-id="7151d-328">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="7151d-329">選取 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-329">Select the **Save** button.</span></span>
1. <span data-ttu-id="7151d-330">在 [開發工具] 區域中，開啟 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="7151d-330">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="7151d-331">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-331">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-332">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-332">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-333">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-333">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-334">將資料夾開啟至路徑 [site] > [wwwroot]。</span><span class="sxs-lookup"><span data-stu-id="7151d-334">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="7151d-335">如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。</span><span class="sxs-lookup"><span data-stu-id="7151d-335">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="7151d-336">如果已提供路徑，請巡覽至記錄檔的位置。</span><span class="sxs-lookup"><span data-stu-id="7151d-336">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="7151d-337">使用檔案名稱旁的鉛筆圖示來開啟記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-337">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="7151d-338">完成疑難排解時，請停用偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-338">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="7151d-339">若要停用增強型偵錯記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="7151d-339">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="7151d-340">從 `<handlerSettings>`web.config*檔案本機移除* 並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-340">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="7151d-341">使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。</span><span class="sxs-lookup"><span data-stu-id="7151d-341">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="7151d-342">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-342">Save the file.</span></span>

<span data-ttu-id="7151d-343">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="7151d-343">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-344">無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-344">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-345">記錄檔大小沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-345">There's no limit on log file size.</span></span> <span data-ttu-id="7151d-346">請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-346">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="7151d-347">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-347">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-348">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-348">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="7151d-349">緩慢或懸掛應用程式（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-349">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="7151d-350">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="7151d-350">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="7151d-351">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-351">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="7151d-352">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="7151d-352">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="7151d-353">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="7151d-353">Monitoring blades</span></span>

<span data-ttu-id="7151d-354">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="7151d-354">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="7151d-355">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-355">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="7151d-356">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="7151d-356">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="7151d-357">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="7151d-357">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="7151d-358">在 [開發工具] 刀鋒視窗區段中，選取 [延伸模組] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-358">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="7151d-359">[ASP.NET Core 延伸模組] 應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="7151d-359">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="7151d-360">如果未安裝這些延伸模組，請選取 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-360">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="7151d-361">從清單中選擇 [ASP.NET Core 延伸模組]。</span><span class="sxs-lookup"><span data-stu-id="7151d-361">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="7151d-362">選取 [確定] 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="7151d-362">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="7151d-363">在 [新增延伸模組] 刀鋒視窗上，選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="7151d-363">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="7151d-364">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="7151d-364">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="7151d-365">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="7151d-365">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="7151d-366">在 Azure 入口網站中，選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-366">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="7151d-367">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-367">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-368">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-368">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-369">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-369">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-370">開啟路徑**網站**的資料夾 > **wwwroot**並向下滾動，以顯示清單底部*的 web.config 檔案*。</span><span class="sxs-lookup"><span data-stu-id="7151d-370">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="7151d-371">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="7151d-371">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="7151d-372">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="7151d-372">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="7151d-373">選取 [儲存] 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-373">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="7151d-374">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-374">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="7151d-375">在 Azure 入口網站中，選取 [診斷記錄檔] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-375">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="7151d-376">選取 [應用程式記錄 (檔案系統)] 和 [詳細錯誤訊息].的 [開啟] 開關。</span><span class="sxs-lookup"><span data-stu-id="7151d-376">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="7151d-377">選取刀鋒視窗頂端的 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-377">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="7151d-378">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤] 的 [開啟] 開關。</span><span class="sxs-lookup"><span data-stu-id="7151d-378">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="7151d-379">選取 [記錄資料流] 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔] 刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="7151d-379">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="7151d-380">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-380">Make a request to the app.</span></span>
1. <span data-ttu-id="7151d-381">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-381">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="7151d-382">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-382">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="7151d-383">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="7151d-383">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="7151d-384">在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-384">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="7151d-385">從資訊看板的 [支援工具] 區域中，選取 [失敗要求追蹤記錄檔]。</span><span class="sxs-lookup"><span data-stu-id="7151d-385">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="7151d-386">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="7151d-386">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="7151d-387">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="7151d-387">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-388">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-388">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-389">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-389">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="7151d-390">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-390">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-391">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-391">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="7151d-392">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-392">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="7151d-393">應用程式事件記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-393">Application Event Log (IIS)</span></span>

<span data-ttu-id="7151d-394">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="7151d-394">Access the Application Event Log:</span></span>

1. <span data-ttu-id="7151d-395">開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-395">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="7151d-396">在 [事件檢視器] 中，開啟 [Windows 記錄] 節點。</span><span class="sxs-lookup"><span data-stu-id="7151d-396">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="7151d-397">選取 [應用程式] 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="7151d-397">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="7151d-398">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-398">Search for errors associated with the failing app.</span></span> <span data-ttu-id="7151d-399">錯誤在 [來源] 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="7151d-399">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="7151d-400">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-400">Run the app at a command prompt</span></span>

<span data-ttu-id="7151d-401">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="7151d-401">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="7151d-402">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-402">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="7151d-403">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="7151d-403">Framework-dependent deployment</span></span>

<span data-ttu-id="7151d-404">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-404">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="7151d-405">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-405">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="7151d-406">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="7151d-406">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="7151d-407">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-407">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="7151d-408">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-408">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="7151d-409">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-409">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="7151d-410">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="7151d-410">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="7151d-411">自封式部署</span><span class="sxs-lookup"><span data-stu-id="7151d-411">Self-contained deployment</span></span>

<span data-ttu-id="7151d-412">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-412">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="7151d-413">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-413">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="7151d-414">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="7151d-414">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="7151d-415">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-415">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="7151d-416">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-416">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="7151d-417">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-417">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="7151d-418">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="7151d-418">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="7151d-419">ASP.NET Core 模組 stdout 記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-419">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="7151d-420">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="7151d-420">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="7151d-421">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-421">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="7151d-422">如果 [logs] 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-422">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="7151d-423">如需有關如何讓 MSBuild 在部署中自動建立 [logs] 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="7151d-423">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="7151d-424">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-424">Edit the *web.config* file.</span></span> <span data-ttu-id="7151d-425">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs] 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="7151d-425">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="7151d-426">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="7151d-426">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="7151d-427">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="7151d-427">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="7151d-428">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="7151d-428">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="7151d-429">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="7151d-429">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="7151d-430">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-430">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="7151d-431">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-431">Make a request to the app.</span></span>
1. <span data-ttu-id="7151d-432">瀏覽至 [logs] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-432">Navigate to the *logs* folder.</span></span> <span data-ttu-id="7151d-433">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-433">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="7151d-434">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-434">Study the log for errors.</span></span>

<span data-ttu-id="7151d-435">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-435">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="7151d-436">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-436">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="7151d-437">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="7151d-437">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="7151d-438">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-438">Save the file.</span></span>

<span data-ttu-id="7151d-439">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="7151d-439">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-440">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-440">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-441">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-441">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="7151d-442">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-442">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-443">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-443">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="7151d-444">ASP.NET Core 模組的調試記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-444">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="7151d-445">將下列處理常式設定新增至應用程式*的 web.config 檔案*，以啟用 ASP.NET Core 模組的 debug 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="7151d-445">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="7151d-446">確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="7151d-446">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="7151d-447">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="7151d-447">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="7151d-448">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="7151d-448">Enable the Developer Exception Page</span></span>

<span data-ttu-id="7151d-449">`ASPNETCORE_ENVIRONMENT`[環境變數可以新增至](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)web.config，以在開發環境中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-449">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="7151d-450">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="7151d-450">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="7151d-451">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="7151d-451">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="7151d-452">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="7151d-452">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="7151d-453">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="7151d-453">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="7151d-454">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="7151d-454">Obtain data from an app</span></span>

<span data-ttu-id="7151d-455">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="7151d-455">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="7151d-456">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="7151d-456">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="7151d-457">緩慢或懸掛應用程式（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-457">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="7151d-458">損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-458">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="7151d-459">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="7151d-459">App crashes or encounters an exception</span></span>

<span data-ttu-id="7151d-460">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="7151d-460">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="7151d-461">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-461">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="7151d-462">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="7151d-462">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="7151d-463">執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="7151d-463">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="7151d-464">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-464">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="7151d-465">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-465">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="7151d-466">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-466">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="7151d-467">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="7151d-467">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="7151d-468">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-468">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="7151d-469">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-469">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="7151d-470">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-470">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="7151d-471">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="7151d-471">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-472">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="7151d-472">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="7151d-473">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="7151d-473">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="7151d-474">當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。</span><span class="sxs-lookup"><span data-stu-id="7151d-474">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="7151d-475">分析傾印</span><span class="sxs-lookup"><span data-stu-id="7151d-475">Analyze the dump</span></span>

<span data-ttu-id="7151d-476">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="7151d-476">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="7151d-477">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="7151d-477">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="7151d-478">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="7151d-478">Clear package caches</span></span>

<span data-ttu-id="7151d-479">升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-479">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="7151d-480">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-480">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="7151d-481">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="7151d-481">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="7151d-482">刪除 [bin] 和 [obj] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-482">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="7151d-483">從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="7151d-483">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="7151d-484">您也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成清除套件快取，並 `nuget locals all -clear`執行命令。</span><span class="sxs-lookup"><span data-stu-id="7151d-484">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="7151d-485">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="7151d-485">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="7151d-486">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="7151d-486">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="7151d-487">重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-487">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7151d-488">其他資源</span><span class="sxs-lookup"><span data-stu-id="7151d-488">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="7151d-489">Azure 文件</span><span class="sxs-lookup"><span data-stu-id="7151d-489">Azure documentation</span></span>

* [<span data-ttu-id="7151d-490">Application Insights for ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7151d-490">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="7151d-491">使用 Visual Studio 在 Azure App Service 中疑難排解 web 應用程式的遠端偵錯程式一節</span><span class="sxs-lookup"><span data-stu-id="7151d-491">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="7151d-492">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="7151d-492">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="7151d-493">作法：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-493">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="7151d-494">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-494">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="7151d-495">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-495">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="7151d-496">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-496">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="7151d-497">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="7151d-497">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="7151d-498">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="7151d-498">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="7151d-499">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="7151d-499">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="7151d-500">Visual Studio 文件</span><span class="sxs-lookup"><span data-stu-id="7151d-500">Visual Studio documentation</span></span>

* [<span data-ttu-id="7151d-501">Visual Studio 2017 中 Azure 上 IIS 的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7151d-501">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="7151d-502">Visual Studio 2017 中遠端 IIS 電腦上的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7151d-502">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="7151d-503">了解使用 Visual Studio 進行偵錯</span><span class="sxs-lookup"><span data-stu-id="7151d-503">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="7151d-504">Visual Studio Code 檔</span><span class="sxs-lookup"><span data-stu-id="7151d-504">Visual Studio Code documentation</span></span>

* <span data-ttu-id="7151d-505">[使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="7151d-505">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="7151d-506">本文提供有關一般應用程式啟動錯誤的資訊，以及如何在將應用程式部署至 Azure App Service 或 IIS 時診斷錯誤的指示：</span><span class="sxs-lookup"><span data-stu-id="7151d-506">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="7151d-507">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="7151d-507">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="7151d-508">說明常見的啟動 HTTP 狀態碼案例。</span><span class="sxs-lookup"><span data-stu-id="7151d-508">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="7151d-509">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-509">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="7151d-510">提供部署至 Azure App Service 之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="7151d-510">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="7151d-511">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-511">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="7151d-512">提供部署至 IIS 或在本機 IIS Express 上執行之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="7151d-512">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="7151d-513">本指南適用于 Windows Server 和 Windows 桌面部署。</span><span class="sxs-lookup"><span data-stu-id="7151d-513">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="7151d-514">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="7151d-514">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="7151d-515">說明當一致封裝在執行主要升級或變更封裝版本時，中斷應用程式時該怎麼辦。</span><span class="sxs-lookup"><span data-stu-id="7151d-515">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="7151d-516">其他資源</span><span class="sxs-lookup"><span data-stu-id="7151d-516">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="7151d-517">列出其他疑難排解主題。</span><span class="sxs-lookup"><span data-stu-id="7151d-517">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="7151d-518">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="7151d-518">App startup errors</span></span>

<span data-ttu-id="7151d-519">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="7151d-519">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="7151d-520">使用本主題中的建議，可以診斷在本機進行偵錯工具時所發生的*502.5-進程失敗*或*500.30 啟動失敗*。</span><span class="sxs-lookup"><span data-stu-id="7151d-520">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="7151d-521">403.14 禁止</span><span class="sxs-lookup"><span data-stu-id="7151d-521">403.14 Forbidden</span></span>

<span data-ttu-id="7151d-522">應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-522">The app fails to start.</span></span> <span data-ttu-id="7151d-523">會記錄下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="7151d-523">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="7151d-524">此錯誤通常是因為主控系統上的部署中斷所造成，其中包括下列任何一種情況：</span><span class="sxs-lookup"><span data-stu-id="7151d-524">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="7151d-525">應用程式會部署到裝載系統上錯誤的資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-525">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="7151d-526">部署進程無法將應用程式的所有檔案和資料夾移到主控系統上的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-526">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="7151d-527">部署中遺漏*web.config*檔案，或*web.config*檔案內容的格式不正確。</span><span class="sxs-lookup"><span data-stu-id="7151d-527">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="7151d-528">執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="7151d-528">Perform the following steps:</span></span>

1. <span data-ttu-id="7151d-529">刪除主控系統上部署資料夾中的所有檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-529">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="7151d-530">使用一般部署方法（例如 Visual Studio、PowerShell 或手動部署），將應用程式的 [*發佈*] 資料夾的內容重新部署至主機系統：</span><span class="sxs-lookup"><span data-stu-id="7151d-530">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="7151d-531">請確認*web.config*檔案存在於部署中，而且其內容正確。</span><span class="sxs-lookup"><span data-stu-id="7151d-531">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="7151d-532">在 Azure App Service 上裝載時，請確認應用程式已部署至 `D:\home\site\wwwroot` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-532">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="7151d-533">當應用程式由 IIS 裝載時，請確認應用程式已部署到 iis**管理員**的 [**基本設定**] 中所顯示的 iis**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="7151d-533">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="7151d-534">藉由比較主控系統上的部署與專案 [*發行*] 資料夾的內容，確認已部署所有應用程式的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-534">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="7151d-535">如需已發佈 ASP.NET Core 應用程式佈建的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="7151d-535">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="7151d-536">如需*web.config*檔案的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="7151d-536">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="7151d-537">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="7151d-537">500 Internal Server Error</span></span>

<span data-ttu-id="7151d-538">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-538">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="7151d-539">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="7151d-539">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="7151d-540">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」的形式出現。</span><span class="sxs-lookup"><span data-stu-id="7151d-540">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="7151d-541">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-541">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="7151d-542">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="7151d-542">From the server's perspective, that's correct.</span></span> <span data-ttu-id="7151d-543">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="7151d-543">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="7151d-544">在伺服器上的命令提示字元中執行應用程式，或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="7151d-544">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="7151d-545">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="7151d-545">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="7151d-546">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-546">The worker process fails.</span></span> <span data-ttu-id="7151d-547">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-547">The app doesn't start.</span></span>

<span data-ttu-id="7151d-548">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)找不到 .NET Core CLR，而且找不到同進程要求處理常式（*aspnetcorev2_inprocess .dll*）。</span><span class="sxs-lookup"><span data-stu-id="7151d-548">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="7151d-549">請確定：</span><span class="sxs-lookup"><span data-stu-id="7151d-549">Check that:</span></span>

* <span data-ttu-id="7151d-550">應用程式以 [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet 套件或 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)為目標。</span><span class="sxs-lookup"><span data-stu-id="7151d-550">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="7151d-551">應用程式設為目標的 ASP.NET Core 共用架構版本有安裝在目標機器上。</span><span class="sxs-lookup"><span data-stu-id="7151d-551">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="7151d-552">500.0 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="7151d-552">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="7151d-553">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-553">The worker process fails.</span></span> <span data-ttu-id="7151d-554">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-554">The app doesn't start.</span></span>

<span data-ttu-id="7151d-555">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)找不到跨進程主控要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="7151d-555">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="7151d-556">請確定 *aspnetcorev2_outofprocess.dll* 出現在子資料夾中，且位於 *aspnetcorev2.dll* 旁。</span><span class="sxs-lookup"><span data-stu-id="7151d-556">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="7151d-557">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="7151d-557">502.5 Process Failure</span></span>

<span data-ttu-id="7151d-558">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-558">The worker process fails.</span></span> <span data-ttu-id="7151d-559">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-559">The app doesn't start.</span></span>

<span data-ttu-id="7151d-560">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-560">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="7151d-561">通常會從應用程式事件記錄檔中的專案和 ASP.NET Core 模組 stdout 記錄檔中判斷進程啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-561">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="7151d-562">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="7151d-562">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="7151d-563">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="7151d-563">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="7151d-564">「共用的架構」是一組安裝於電腦上且由  *之類的中繼套件所參考的組件 (* .dll`Microsoft.AspNetCore.App` 檔案)。</span><span class="sxs-lookup"><span data-stu-id="7151d-564">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="7151d-565">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="7151d-565">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="7151d-566">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="7151d-566">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="7151d-567">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗] 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="7151d-567">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="7151d-568">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="7151d-568">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="7151d-569">應用程式無法啟動，因為無法載入應用程式的組件 ( *.dll*)。</span><span class="sxs-lookup"><span data-stu-id="7151d-569">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="7151d-570">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-570">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="7151d-571">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="7151d-571">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="7151d-572">在 IIS 管理員的 [應用程式集區] 中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="7151d-572">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="7151d-573">在 [動作] 面板中，選取 [編輯應用程式集區] 下的 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="7151d-573">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="7151d-574">設定 [啟用 32 位元應用程式]：</span><span class="sxs-lookup"><span data-stu-id="7151d-574">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="7151d-575">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="7151d-575">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="7151d-576">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="7151d-576">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="7151d-577">確認專案檔中的 `<Platform>` MSBuild 屬性和應用程式的已發佈位之間沒有衝突。</span><span class="sxs-lookup"><span data-stu-id="7151d-577">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="7151d-578">連線重設</span><span class="sxs-lookup"><span data-stu-id="7151d-578">Connection reset</span></span>

<span data-ttu-id="7151d-579">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="7151d-579">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="7151d-580">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-580">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="7151d-581">這類錯誤會在用戶端上顯示為「連線重設」錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-581">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="7151d-582">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="7151d-582">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="7151d-583">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="7151d-583">Default startup limits</span></span>

<span data-ttu-id="7151d-584">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是以120秒的預設*startupTimeLimit*來設定。</span><span class="sxs-lookup"><span data-stu-id="7151d-584">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="7151d-585">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-585">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="7151d-586">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="7151d-586">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="7151d-587">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-587">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="7151d-588">應用程式事件記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-588">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="7151d-589">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題] 刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="7151d-589">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="7151d-590">在 Azure 入口網站的 [應用程式服務] 中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-590">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="7151d-591">選取 [診斷並解決問題]。</span><span class="sxs-lookup"><span data-stu-id="7151d-591">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="7151d-592">選取 [診斷工具] 標題。</span><span class="sxs-lookup"><span data-stu-id="7151d-592">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="7151d-593">在 [支援工具] 下，選取 [應用程式事件] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-593">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="7151d-594">檢查 [來源] 資料行中 *IIS AspNetCoreModule* 或 **IIS AspNetCoreModule V2** 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-594">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="7151d-595">除了使用 [診斷並解決問題] 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="7151d-595">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="7151d-596">在 [開發工具] 區域中，開啟 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="7151d-596">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="7151d-597">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-597">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-598">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-598">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-599">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-599">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-600">開啟 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-600">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="7151d-601">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="7151d-601">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="7151d-602">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-602">Examine the log.</span></span> <span data-ttu-id="7151d-603">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="7151d-603">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="7151d-604">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-604">Run the app in the Kudu console</span></span>

<span data-ttu-id="7151d-605">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="7151d-605">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="7151d-606">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="7151d-606">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="7151d-607">在 [開發工具] 區域中，開啟 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="7151d-607">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="7151d-608">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-608">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-609">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-609">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-610">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-610">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="7151d-611">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-611">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="7151d-612">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="7151d-612">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="7151d-613">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="7151d-613">Run the app:</span></span>
   * <span data-ttu-id="7151d-614">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-614">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="7151d-615">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-615">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="7151d-616">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-616">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="7151d-617">**在預覽版本上執行的 Framework 相依部署**</span><span class="sxs-lookup"><span data-stu-id="7151d-617">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="7151d-618">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="7151d-618">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="7151d-619">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="7151d-619">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="7151d-620">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="7151d-620">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="7151d-621">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-621">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="7151d-622">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-622">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="7151d-623">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="7151d-623">**Current release**</span></span>

* <span data-ttu-id="7151d-624">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-624">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="7151d-625">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="7151d-625">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="7151d-626">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-626">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="7151d-627">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="7151d-627">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="7151d-628">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-628">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="7151d-629">**在預覽版本上執行的 Framework 相依部署**</span><span class="sxs-lookup"><span data-stu-id="7151d-629">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="7151d-630">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="7151d-630">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="7151d-631">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="7151d-631">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="7151d-632">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="7151d-632">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="7151d-633">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-633">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="7151d-634">ASP.NET Core 模組 stdout 記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-634">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="7151d-635">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="7151d-635">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="7151d-636">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="7151d-636">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="7151d-637">在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-637">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="7151d-638">在 [選取問題類別] 底下，選取 [Web 應用程式停止運作] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-638">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="7151d-639">在 **建議的解決方案** 底下 >**啟用 Stdout 記錄**檔重新導向，選取  按鈕以**開啟 Kudu 主控台 以編輯 web.config**。</span><span class="sxs-lookup"><span data-stu-id="7151d-639">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="7151d-640">在 Kudu [診斷主控台] 中，將資料夾開啟至路徑 [site] > [wwwroot]。</span><span class="sxs-lookup"><span data-stu-id="7151d-640">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="7151d-641">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-641">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="7151d-642">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="7151d-642">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="7151d-643">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="7151d-643">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="7151d-644">選取 [儲存] 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-644">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="7151d-645">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-645">Make a request to the app.</span></span>
1. <span data-ttu-id="7151d-646">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="7151d-646">Return to the Azure portal.</span></span> <span data-ttu-id="7151d-647">選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-647">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="7151d-648">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-648">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-649">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-649">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-650">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-650">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-651">選取 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-651">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="7151d-652">檢查 [修改時間] 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-652">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="7151d-653">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-653">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="7151d-654">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-654">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="7151d-655">在 Kudu [診斷主控台] 中，返回路徑 [site] > [wwwroot] 以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-655">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="7151d-656">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-656">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="7151d-657">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="7151d-657">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="7151d-658">選取 [儲存] 以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-658">Select **Save** to save the file.</span></span>

<span data-ttu-id="7151d-659">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="7151d-659">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-660">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-660">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-661">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-661">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="7151d-662">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-662">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="7151d-663">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-663">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-664">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-664">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="7151d-665">ASP.NET Core 模組的 debug 記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-665">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="7151d-666">ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-666">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="7151d-667">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="7151d-667">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="7151d-668">若要啟用增強型診斷記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="7151d-668">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="7151d-669">遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-669">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="7151d-670">重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-670">Redeploy the app.</span></span>
   * <span data-ttu-id="7151d-671">使用 Kudu 主控台，將`<handlerSettings>`增強型診斷記錄[中所顯示的 ](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) 新增至即時應用程式的 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="7151d-671">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="7151d-672">在 [開發工具] 區域中，開啟 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="7151d-672">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="7151d-673">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-673">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-674">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-674">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="7151d-675">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-675">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="7151d-676">將資料夾開啟至路徑 [site] > [wwwroot]。</span><span class="sxs-lookup"><span data-stu-id="7151d-676">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="7151d-677">選取鉛筆圖示來編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-677">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="7151d-678">新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。</span><span class="sxs-lookup"><span data-stu-id="7151d-678">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="7151d-679">選取 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-679">Select the **Save** button.</span></span>
1. <span data-ttu-id="7151d-680">在 [開發工具] 區域中，開啟 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="7151d-680">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="7151d-681">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-681">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-682">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-682">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-683">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-683">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-684">將資料夾開啟至路徑 [site] > [wwwroot]。</span><span class="sxs-lookup"><span data-stu-id="7151d-684">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="7151d-685">如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。</span><span class="sxs-lookup"><span data-stu-id="7151d-685">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="7151d-686">如果已提供路徑，請巡覽至記錄檔的位置。</span><span class="sxs-lookup"><span data-stu-id="7151d-686">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="7151d-687">使用檔案名稱旁的鉛筆圖示來開啟記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-687">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="7151d-688">完成疑難排解時，請停用偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-688">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="7151d-689">若要停用增強型偵錯記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="7151d-689">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="7151d-690">從 `<handlerSettings>`web.config*檔案本機移除* 並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-690">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="7151d-691">使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。</span><span class="sxs-lookup"><span data-stu-id="7151d-691">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="7151d-692">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-692">Save the file.</span></span>

<span data-ttu-id="7151d-693">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="7151d-693">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-694">無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-694">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-695">記錄檔大小沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-695">There's no limit on log file size.</span></span> <span data-ttu-id="7151d-696">請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-696">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="7151d-697">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-697">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-698">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-698">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="7151d-699">緩慢或懸掛應用程式（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-699">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="7151d-700">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="7151d-700">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="7151d-701">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-701">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="7151d-702">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="7151d-702">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="7151d-703">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="7151d-703">Monitoring blades</span></span>

<span data-ttu-id="7151d-704">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="7151d-704">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="7151d-705">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-705">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="7151d-706">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="7151d-706">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="7151d-707">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="7151d-707">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="7151d-708">在 [開發工具] 刀鋒視窗區段中，選取 [延伸模組] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-708">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="7151d-709">[ASP.NET Core 延伸模組] 應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="7151d-709">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="7151d-710">如果未安裝這些延伸模組，請選取 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-710">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="7151d-711">從清單中選擇 [ASP.NET Core 延伸模組]。</span><span class="sxs-lookup"><span data-stu-id="7151d-711">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="7151d-712">選取 [確定] 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="7151d-712">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="7151d-713">在 [新增延伸模組] 刀鋒視窗上，選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="7151d-713">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="7151d-714">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="7151d-714">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="7151d-715">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="7151d-715">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="7151d-716">在 Azure 入口網站中，選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-716">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="7151d-717">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-717">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-718">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-718">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-719">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-719">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-720">開啟路徑**網站**的資料夾 > **wwwroot**並向下滾動，以顯示清單底部*的 web.config 檔案*。</span><span class="sxs-lookup"><span data-stu-id="7151d-720">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="7151d-721">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="7151d-721">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="7151d-722">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="7151d-722">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="7151d-723">選取 [儲存] 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-723">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="7151d-724">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-724">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="7151d-725">在 Azure 入口網站中，選取 [診斷記錄檔] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-725">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="7151d-726">選取 [應用程式記錄 (檔案系統)] 和 [詳細錯誤訊息].的 [開啟] 開關。</span><span class="sxs-lookup"><span data-stu-id="7151d-726">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="7151d-727">選取刀鋒視窗頂端的 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-727">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="7151d-728">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤] 的 [開啟] 開關。</span><span class="sxs-lookup"><span data-stu-id="7151d-728">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="7151d-729">選取 [記錄資料流] 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔] 刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="7151d-729">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="7151d-730">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-730">Make a request to the app.</span></span>
1. <span data-ttu-id="7151d-731">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-731">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="7151d-732">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-732">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="7151d-733">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="7151d-733">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="7151d-734">在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-734">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="7151d-735">從資訊看板的 [支援工具] 區域中，選取 [失敗要求追蹤記錄檔]。</span><span class="sxs-lookup"><span data-stu-id="7151d-735">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="7151d-736">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="7151d-736">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="7151d-737">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="7151d-737">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-738">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-738">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-739">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-739">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="7151d-740">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-740">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-741">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-741">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="7151d-742">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-742">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="7151d-743">應用程式事件記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-743">Application Event Log (IIS)</span></span>

<span data-ttu-id="7151d-744">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="7151d-744">Access the Application Event Log:</span></span>

1. <span data-ttu-id="7151d-745">開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-745">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="7151d-746">在 [事件檢視器] 中，開啟 [Windows 記錄] 節點。</span><span class="sxs-lookup"><span data-stu-id="7151d-746">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="7151d-747">選取 [應用程式] 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="7151d-747">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="7151d-748">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-748">Search for errors associated with the failing app.</span></span> <span data-ttu-id="7151d-749">錯誤在 [來源] 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="7151d-749">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="7151d-750">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-750">Run the app at a command prompt</span></span>

<span data-ttu-id="7151d-751">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="7151d-751">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="7151d-752">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-752">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="7151d-753">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="7151d-753">Framework-dependent deployment</span></span>

<span data-ttu-id="7151d-754">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-754">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="7151d-755">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-755">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="7151d-756">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="7151d-756">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="7151d-757">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-757">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="7151d-758">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-758">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="7151d-759">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-759">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="7151d-760">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="7151d-760">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="7151d-761">自封式部署</span><span class="sxs-lookup"><span data-stu-id="7151d-761">Self-contained deployment</span></span>

<span data-ttu-id="7151d-762">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-762">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="7151d-763">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-763">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="7151d-764">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="7151d-764">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="7151d-765">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-765">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="7151d-766">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-766">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="7151d-767">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-767">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="7151d-768">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="7151d-768">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="7151d-769">ASP.NET Core 模組 stdout 記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-769">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="7151d-770">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="7151d-770">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="7151d-771">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-771">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="7151d-772">如果 [logs] 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-772">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="7151d-773">如需有關如何讓 MSBuild 在部署中自動建立 [logs] 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="7151d-773">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="7151d-774">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-774">Edit the *web.config* file.</span></span> <span data-ttu-id="7151d-775">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs] 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="7151d-775">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="7151d-776">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="7151d-776">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="7151d-777">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="7151d-777">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="7151d-778">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="7151d-778">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="7151d-779">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="7151d-779">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="7151d-780">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-780">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="7151d-781">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-781">Make a request to the app.</span></span>
1. <span data-ttu-id="7151d-782">瀏覽至 [logs] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-782">Navigate to the *logs* folder.</span></span> <span data-ttu-id="7151d-783">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-783">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="7151d-784">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-784">Study the log for errors.</span></span>

<span data-ttu-id="7151d-785">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-785">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="7151d-786">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-786">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="7151d-787">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="7151d-787">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="7151d-788">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-788">Save the file.</span></span>

<span data-ttu-id="7151d-789">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="7151d-789">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-790">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-790">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-791">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-791">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="7151d-792">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-792">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-793">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-793">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="7151d-794">ASP.NET Core 模組的調試記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-794">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="7151d-795">將下列處理常式設定新增至應用程式*的 web.config 檔案*，以啟用 ASP.NET Core 模組的 debug 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="7151d-795">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="7151d-796">確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="7151d-796">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="7151d-797">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="7151d-797">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="7151d-798">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="7151d-798">Enable the Developer Exception Page</span></span>

<span data-ttu-id="7151d-799">`ASPNETCORE_ENVIRONMENT`[環境變數可以新增至](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)web.config，以在開發環境中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-799">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="7151d-800">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="7151d-800">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="7151d-801">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="7151d-801">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="7151d-802">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="7151d-802">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="7151d-803">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="7151d-803">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="7151d-804">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="7151d-804">Obtain data from an app</span></span>

<span data-ttu-id="7151d-805">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="7151d-805">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="7151d-806">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="7151d-806">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="7151d-807">緩慢或懸掛應用程式（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-807">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="7151d-808">損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-808">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="7151d-809">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="7151d-809">App crashes or encounters an exception</span></span>

<span data-ttu-id="7151d-810">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="7151d-810">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="7151d-811">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-811">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="7151d-812">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="7151d-812">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="7151d-813">執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="7151d-813">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="7151d-814">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-814">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="7151d-815">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-815">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="7151d-816">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-816">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="7151d-817">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="7151d-817">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="7151d-818">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-818">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="7151d-819">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-819">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="7151d-820">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-820">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="7151d-821">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="7151d-821">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-822">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="7151d-822">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="7151d-823">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="7151d-823">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="7151d-824">當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。</span><span class="sxs-lookup"><span data-stu-id="7151d-824">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="7151d-825">分析傾印</span><span class="sxs-lookup"><span data-stu-id="7151d-825">Analyze the dump</span></span>

<span data-ttu-id="7151d-826">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="7151d-826">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="7151d-827">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="7151d-827">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="7151d-828">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="7151d-828">Clear package caches</span></span>

<span data-ttu-id="7151d-829">升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-829">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="7151d-830">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-830">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="7151d-831">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="7151d-831">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="7151d-832">刪除 [bin] 和 [obj] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-832">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="7151d-833">從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="7151d-833">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="7151d-834">您也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成清除套件快取，並 `nuget locals all -clear`執行命令。</span><span class="sxs-lookup"><span data-stu-id="7151d-834">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="7151d-835">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="7151d-835">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="7151d-836">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="7151d-836">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="7151d-837">重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-837">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7151d-838">其他資源</span><span class="sxs-lookup"><span data-stu-id="7151d-838">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="7151d-839">Azure 文件</span><span class="sxs-lookup"><span data-stu-id="7151d-839">Azure documentation</span></span>

* [<span data-ttu-id="7151d-840">Application Insights for ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7151d-840">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="7151d-841">使用 Visual Studio 在 Azure App Service 中疑難排解 web 應用程式的遠端偵錯程式一節</span><span class="sxs-lookup"><span data-stu-id="7151d-841">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="7151d-842">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="7151d-842">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="7151d-843">作法：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-843">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="7151d-844">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-844">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="7151d-845">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-845">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="7151d-846">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-846">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="7151d-847">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="7151d-847">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="7151d-848">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="7151d-848">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="7151d-849">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="7151d-849">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="7151d-850">Visual Studio 文件</span><span class="sxs-lookup"><span data-stu-id="7151d-850">Visual Studio documentation</span></span>

* [<span data-ttu-id="7151d-851">Visual Studio 2017 中 Azure 上 IIS 的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7151d-851">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="7151d-852">Visual Studio 2017 中遠端 IIS 電腦上的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7151d-852">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="7151d-853">了解使用 Visual Studio 進行偵錯</span><span class="sxs-lookup"><span data-stu-id="7151d-853">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="7151d-854">Visual Studio Code 檔</span><span class="sxs-lookup"><span data-stu-id="7151d-854">Visual Studio Code documentation</span></span>

* <span data-ttu-id="7151d-855">[使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="7151d-855">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7151d-856">本文提供有關一般應用程式啟動錯誤的資訊，以及如何在將應用程式部署至 Azure App Service 或 IIS 時診斷錯誤的指示：</span><span class="sxs-lookup"><span data-stu-id="7151d-856">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="7151d-857">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="7151d-857">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="7151d-858">說明常見的啟動 HTTP 狀態碼案例。</span><span class="sxs-lookup"><span data-stu-id="7151d-858">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="7151d-859">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-859">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="7151d-860">提供部署至 Azure App Service 之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="7151d-860">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="7151d-861">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-861">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="7151d-862">提供部署至 IIS 或在本機 IIS Express 上執行之應用程式的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="7151d-862">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="7151d-863">本指南適用于 Windows Server 和 Windows 桌面部署。</span><span class="sxs-lookup"><span data-stu-id="7151d-863">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="7151d-864">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="7151d-864">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="7151d-865">說明當一致封裝在執行主要升級或變更封裝版本時，中斷應用程式時該怎麼辦。</span><span class="sxs-lookup"><span data-stu-id="7151d-865">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="7151d-866">其他資源</span><span class="sxs-lookup"><span data-stu-id="7151d-866">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="7151d-867">列出其他疑難排解主題。</span><span class="sxs-lookup"><span data-stu-id="7151d-867">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="7151d-868">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="7151d-868">App startup errors</span></span>

<span data-ttu-id="7151d-869">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="7151d-869">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="7151d-870">使用本主題中的建議，可以診斷本機上的偵錯工具發生的*502.5 進程失敗*。</span><span class="sxs-lookup"><span data-stu-id="7151d-870">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="7151d-871">403.14 禁止</span><span class="sxs-lookup"><span data-stu-id="7151d-871">403.14 Forbidden</span></span>

<span data-ttu-id="7151d-872">應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-872">The app fails to start.</span></span> <span data-ttu-id="7151d-873">會記錄下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="7151d-873">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="7151d-874">此錯誤通常是因為主控系統上的部署中斷所造成，其中包括下列任何一種情況：</span><span class="sxs-lookup"><span data-stu-id="7151d-874">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="7151d-875">應用程式會部署到裝載系統上錯誤的資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-875">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="7151d-876">部署進程無法將應用程式的所有檔案和資料夾移到主控系統上的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-876">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="7151d-877">部署中遺漏*web.config*檔案，或*web.config*檔案內容的格式不正確。</span><span class="sxs-lookup"><span data-stu-id="7151d-877">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="7151d-878">執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="7151d-878">Perform the following steps:</span></span>

1. <span data-ttu-id="7151d-879">刪除主控系統上部署資料夾中的所有檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-879">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="7151d-880">使用一般部署方法（例如 Visual Studio、PowerShell 或手動部署），將應用程式的 [*發佈*] 資料夾的內容重新部署至主機系統：</span><span class="sxs-lookup"><span data-stu-id="7151d-880">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="7151d-881">請確認*web.config*檔案存在於部署中，而且其內容正確。</span><span class="sxs-lookup"><span data-stu-id="7151d-881">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="7151d-882">在 Azure App Service 上裝載時，請確認應用程式已部署至 `D:\home\site\wwwroot` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-882">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="7151d-883">當應用程式由 IIS 裝載時，請確認應用程式已部署到 iis**管理員**的 [**基本設定**] 中所顯示的 iis**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="7151d-883">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="7151d-884">藉由比較主控系統上的部署與專案 [*發行*] 資料夾的內容，確認已部署所有應用程式的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-884">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="7151d-885">如需已發佈 ASP.NET Core 應用程式佈建的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="7151d-885">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="7151d-886">如需*web.config*檔案的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="7151d-886">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="7151d-887">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="7151d-887">500 Internal Server Error</span></span>

<span data-ttu-id="7151d-888">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-888">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="7151d-889">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="7151d-889">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="7151d-890">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」的形式出現。</span><span class="sxs-lookup"><span data-stu-id="7151d-890">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="7151d-891">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-891">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="7151d-892">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="7151d-892">From the server's perspective, that's correct.</span></span> <span data-ttu-id="7151d-893">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="7151d-893">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="7151d-894">在伺服器上的命令提示字元中執行應用程式，或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="7151d-894">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="7151d-895">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="7151d-895">502.5 Process Failure</span></span>

<span data-ttu-id="7151d-896">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-896">The worker process fails.</span></span> <span data-ttu-id="7151d-897">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-897">The app doesn't start.</span></span>

<span data-ttu-id="7151d-898">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-898">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="7151d-899">通常會從應用程式事件記錄檔中的專案和 ASP.NET Core 模組 stdout 記錄檔中判斷進程啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-899">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="7151d-900">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="7151d-900">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="7151d-901">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="7151d-901">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="7151d-902">「共用的架構」是一組安裝於電腦上且由  *之類的中繼套件所參考的組件 (* .dll`Microsoft.AspNetCore.App` 檔案)。</span><span class="sxs-lookup"><span data-stu-id="7151d-902">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="7151d-903">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="7151d-903">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="7151d-904">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="7151d-904">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="7151d-905">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗] 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="7151d-905">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="7151d-906">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="7151d-906">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="7151d-907">應用程式無法啟動，因為無法載入應用程式的組件 ( *.dll*)。</span><span class="sxs-lookup"><span data-stu-id="7151d-907">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="7151d-908">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-908">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="7151d-909">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="7151d-909">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="7151d-910">在 IIS 管理員的 [應用程式集區] 中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="7151d-910">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="7151d-911">在 [動作] 面板中，選取 [編輯應用程式集區] 下的 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="7151d-911">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="7151d-912">設定 [啟用 32 位元應用程式]：</span><span class="sxs-lookup"><span data-stu-id="7151d-912">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="7151d-913">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="7151d-913">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="7151d-914">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="7151d-914">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="7151d-915">確認專案檔中的 `<Platform>` MSBuild 屬性和應用程式的已發佈位之間沒有衝突。</span><span class="sxs-lookup"><span data-stu-id="7151d-915">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="7151d-916">連線重設</span><span class="sxs-lookup"><span data-stu-id="7151d-916">Connection reset</span></span>

<span data-ttu-id="7151d-917">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」。</span><span class="sxs-lookup"><span data-stu-id="7151d-917">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="7151d-918">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-918">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="7151d-919">這類錯誤會在用戶端上顯示為「連線重設」錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-919">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="7151d-920">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="7151d-920">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="7151d-921">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="7151d-921">Default startup limits</span></span>

<span data-ttu-id="7151d-922">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是以120秒的預設*startupTimeLimit*來設定。</span><span class="sxs-lookup"><span data-stu-id="7151d-922">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="7151d-923">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="7151d-923">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="7151d-924">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="7151d-924">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="7151d-925">針對 Azure App Service 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-925">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="7151d-926">應用程式事件記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-926">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="7151d-927">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題] 刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="7151d-927">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="7151d-928">在 Azure 入口網站的 [應用程式服務] 中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-928">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="7151d-929">選取 [診斷並解決問題]。</span><span class="sxs-lookup"><span data-stu-id="7151d-929">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="7151d-930">選取 [診斷工具] 標題。</span><span class="sxs-lookup"><span data-stu-id="7151d-930">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="7151d-931">在 [支援工具] 下，選取 [應用程式事件] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-931">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="7151d-932">檢查 [來源] 資料行中 *IIS AspNetCoreModule* 或 **IIS AspNetCoreModule V2** 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-932">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="7151d-933">除了使用 [診斷並解決問題] 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="7151d-933">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="7151d-934">在 [開發工具] 區域中，開啟 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="7151d-934">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="7151d-935">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-935">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-936">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-936">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-937">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-937">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-938">開啟 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-938">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="7151d-939">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="7151d-939">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="7151d-940">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-940">Examine the log.</span></span> <span data-ttu-id="7151d-941">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="7151d-941">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="7151d-942">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-942">Run the app in the Kudu console</span></span>

<span data-ttu-id="7151d-943">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="7151d-943">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="7151d-944">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="7151d-944">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="7151d-945">在 [開發工具] 區域中，開啟 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="7151d-945">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="7151d-946">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-946">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-947">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-947">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-948">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-948">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="7151d-949">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-949">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="7151d-950">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="7151d-950">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="7151d-951">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="7151d-951">Run the app:</span></span>
   * <span data-ttu-id="7151d-952">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-952">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="7151d-953">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-953">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="7151d-954">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-954">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="7151d-955">**在預覽版本上執行的 Framework 相依部署**</span><span class="sxs-lookup"><span data-stu-id="7151d-955">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="7151d-956">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="7151d-956">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="7151d-957">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="7151d-957">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="7151d-958">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="7151d-958">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="7151d-959">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-959">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="7151d-960">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-960">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="7151d-961">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="7151d-961">**Current release**</span></span>

* <span data-ttu-id="7151d-962">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-962">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="7151d-963">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="7151d-963">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="7151d-964">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-964">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="7151d-965">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="7151d-965">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="7151d-966">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-966">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="7151d-967">**在預覽版本上執行的 Framework 相依部署**</span><span class="sxs-lookup"><span data-stu-id="7151d-967">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="7151d-968">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="7151d-968">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="7151d-969">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="7151d-969">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="7151d-970">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="7151d-970">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="7151d-971">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="7151d-971">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="7151d-972">ASP.NET Core 模組 stdout 記錄檔（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-972">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="7151d-973">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="7151d-973">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="7151d-974">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="7151d-974">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="7151d-975">在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-975">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="7151d-976">在 [選取問題類別] 底下，選取 [Web 應用程式停止運作] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-976">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="7151d-977">在 **建議的解決方案** 底下 >**啟用 Stdout 記錄**檔重新導向，選取  按鈕以**開啟 Kudu 主控台 以編輯 web.config**。</span><span class="sxs-lookup"><span data-stu-id="7151d-977">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="7151d-978">在 Kudu [診斷主控台] 中，將資料夾開啟至路徑 [site] > [wwwroot]。</span><span class="sxs-lookup"><span data-stu-id="7151d-978">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="7151d-979">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-979">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="7151d-980">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="7151d-980">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="7151d-981">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="7151d-981">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="7151d-982">選取 [儲存] 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-982">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="7151d-983">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-983">Make a request to the app.</span></span>
1. <span data-ttu-id="7151d-984">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="7151d-984">Return to the Azure portal.</span></span> <span data-ttu-id="7151d-985">選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-985">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="7151d-986">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-986">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-987">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-987">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-988">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-988">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-989">選取 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-989">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="7151d-990">檢查 [修改時間] 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-990">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="7151d-991">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-991">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="7151d-992">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-992">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="7151d-993">在 Kudu [診斷主控台] 中，返回路徑 [site] > [wwwroot] 以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-993">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="7151d-994">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-994">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="7151d-995">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="7151d-995">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="7151d-996">選取 [儲存] 以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-996">Select **Save** to save the file.</span></span>

<span data-ttu-id="7151d-997">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="7151d-997">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-998">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-998">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-999">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-999">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="7151d-1000">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-1000">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="7151d-1001">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-1001">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-1002">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1002">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="7151d-1003">緩慢或懸掛應用程式（Azure App Service）</span><span class="sxs-lookup"><span data-stu-id="7151d-1003">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="7151d-1004">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="7151d-1004">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="7151d-1005">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-1005">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="7151d-1006">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="7151d-1006">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="7151d-1007">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="7151d-1007">Monitoring blades</span></span>

<span data-ttu-id="7151d-1008">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="7151d-1008">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="7151d-1009">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-1009">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="7151d-1010">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="7151d-1010">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="7151d-1011">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="7151d-1011">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="7151d-1012">在 [開發工具] 刀鋒視窗區段中，選取 [延伸模組] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-1012">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="7151d-1013">[ASP.NET Core 延伸模組] 應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="7151d-1013">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="7151d-1014">如果未安裝這些延伸模組，請選取 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-1014">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="7151d-1015">從清單中選擇 [ASP.NET Core 延伸模組]。</span><span class="sxs-lookup"><span data-stu-id="7151d-1015">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="7151d-1016">選取 [確定] 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="7151d-1016">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="7151d-1017">在 [新增延伸模組] 刀鋒視窗上，選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="7151d-1017">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="7151d-1018">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="7151d-1018">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="7151d-1019">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="7151d-1019">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="7151d-1020">在 Azure 入口網站中，選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-1020">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="7151d-1021">選取 [執行 **]&rarr;** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-1021">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="7151d-1022">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="7151d-1022">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="7151d-1023">使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。</span><span class="sxs-lookup"><span data-stu-id="7151d-1023">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="7151d-1024">開啟路徑**網站**的資料夾 > **wwwroot**並向下滾動，以顯示清單底部*的 web.config 檔案*。</span><span class="sxs-lookup"><span data-stu-id="7151d-1024">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="7151d-1025">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="7151d-1025">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="7151d-1026">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="7151d-1026">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="7151d-1027">選取 [儲存] 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-1027">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="7151d-1028">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-1028">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="7151d-1029">在 Azure 入口網站中，選取 [診斷記錄檔] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-1029">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="7151d-1030">選取 [應用程式記錄 (檔案系統)] 和 [詳細錯誤訊息].的 [開啟] 開關。</span><span class="sxs-lookup"><span data-stu-id="7151d-1030">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="7151d-1031">選取刀鋒視窗頂端的 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7151d-1031">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="7151d-1032">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤] 的 [開啟] 開關。</span><span class="sxs-lookup"><span data-stu-id="7151d-1032">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="7151d-1033">選取 [記錄資料流] 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔] 刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1033">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="7151d-1034">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-1034">Make a request to the app.</span></span>
1. <span data-ttu-id="7151d-1035">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-1035">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="7151d-1036">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="7151d-1036">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="7151d-1037">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="7151d-1037">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="7151d-1038">在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-1038">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="7151d-1039">從資訊看板的 [支援工具] 區域中，選取 [失敗要求追蹤記錄檔]。</span><span class="sxs-lookup"><span data-stu-id="7151d-1039">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="7151d-1040">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1040">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="7151d-1041">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1041">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-1042">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-1042">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-1043">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-1043">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="7151d-1044">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-1044">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-1045">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1045">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="7151d-1046">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-1046">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="7151d-1047">應用程式事件記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-1047">Application Event Log (IIS)</span></span>

<span data-ttu-id="7151d-1048">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="7151d-1048">Access the Application Event Log:</span></span>

1. <span data-ttu-id="7151d-1049">開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-1049">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="7151d-1050">在 [事件檢視器] 中，開啟 [Windows 記錄] 節點。</span><span class="sxs-lookup"><span data-stu-id="7151d-1050">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="7151d-1051">選取 [應用程式] 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="7151d-1051">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="7151d-1052">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-1052">Search for errors associated with the failing app.</span></span> <span data-ttu-id="7151d-1053">錯誤在 [來源] 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="7151d-1053">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="7151d-1054">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-1054">Run the app at a command prompt</span></span>

<span data-ttu-id="7151d-1055">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="7151d-1055">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="7151d-1056">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-1056">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="7151d-1057">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="7151d-1057">Framework-dependent deployment</span></span>

<span data-ttu-id="7151d-1058">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-1058">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="7151d-1059">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-1059">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="7151d-1060">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="7151d-1060">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="7151d-1061">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-1061">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="7151d-1062">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-1062">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="7151d-1063">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-1063">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="7151d-1064">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="7151d-1064">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="7151d-1065">自封式部署</span><span class="sxs-lookup"><span data-stu-id="7151d-1065">Self-contained deployment</span></span>

<span data-ttu-id="7151d-1066">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="7151d-1066">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="7151d-1067">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-1067">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="7151d-1068">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="7151d-1068">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="7151d-1069">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="7151d-1069">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="7151d-1070">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-1070">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="7151d-1071">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-1071">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="7151d-1072">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="7151d-1072">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="7151d-1073">ASP.NET Core 模組 stdout 記錄檔（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-1073">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="7151d-1074">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="7151d-1074">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="7151d-1075">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-1075">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="7151d-1076">如果 [logs] 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-1076">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="7151d-1077">如需有關如何讓 MSBuild 在部署中自動建立 [logs] 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="7151d-1077">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="7151d-1078">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-1078">Edit the *web.config* file.</span></span> <span data-ttu-id="7151d-1079">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs] 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1079">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="7151d-1080">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="7151d-1080">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="7151d-1081">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="7151d-1081">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="7151d-1082">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="7151d-1082">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="7151d-1083">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="7151d-1083">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="7151d-1084">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-1084">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="7151d-1085">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="7151d-1085">Make a request to the app.</span></span>
1. <span data-ttu-id="7151d-1086">瀏覽至 [logs] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-1086">Navigate to the *logs* folder.</span></span> <span data-ttu-id="7151d-1087">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7151d-1087">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="7151d-1088">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="7151d-1088">Study the log for errors.</span></span>

<span data-ttu-id="7151d-1089">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="7151d-1089">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="7151d-1090">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-1090">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="7151d-1091">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="7151d-1091">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="7151d-1092">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-1092">Save the file.</span></span>

<span data-ttu-id="7151d-1093">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="7151d-1093">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-1094">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-1094">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="7151d-1095">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="7151d-1095">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="7151d-1096">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="7151d-1096">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="7151d-1097">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1097">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="7151d-1098">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="7151d-1098">Enable the Developer Exception Page</span></span>

<span data-ttu-id="7151d-1099">`ASPNETCORE_ENVIRONMENT`[環境變數可以新增至](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)web.config，以在開發環境中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-1099">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="7151d-1100">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1100">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="7151d-1101">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="7151d-1101">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="7151d-1102">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="7151d-1102">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="7151d-1103">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1103">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="7151d-1104">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="7151d-1104">Obtain data from an app</span></span>

<span data-ttu-id="7151d-1105">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="7151d-1105">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="7151d-1106">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="7151d-1106">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="7151d-1107">緩慢或懸掛應用程式（IIS）</span><span class="sxs-lookup"><span data-stu-id="7151d-1107">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="7151d-1108">損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="7151d-1108">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="7151d-1109">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="7151d-1109">App crashes or encounters an exception</span></span>

<span data-ttu-id="7151d-1110">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="7151d-1110">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="7151d-1111">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-1111">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="7151d-1112">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="7151d-1112">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="7151d-1113">執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="7151d-1113">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="7151d-1114">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-1114">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="7151d-1115">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-1115">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="7151d-1116">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-1116">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="7151d-1117">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="7151d-1117">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="7151d-1118">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-1118">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="7151d-1119">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="7151d-1119">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="7151d-1120">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-1120">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="7151d-1121">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="7151d-1121">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="7151d-1122">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1122">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="7151d-1123">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="7151d-1123">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="7151d-1124">當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。</span><span class="sxs-lookup"><span data-stu-id="7151d-1124">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="7151d-1125">分析傾印</span><span class="sxs-lookup"><span data-stu-id="7151d-1125">Analyze the dump</span></span>

<span data-ttu-id="7151d-1126">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="7151d-1126">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="7151d-1127">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="7151d-1127">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="7151d-1128">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="7151d-1128">Clear package caches</span></span>

<span data-ttu-id="7151d-1129">升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="7151d-1129">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="7151d-1130">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="7151d-1130">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="7151d-1131">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="7151d-1131">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="7151d-1132">刪除 [bin] 和 [obj] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7151d-1132">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="7151d-1133">從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="7151d-1133">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="7151d-1134">您也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成清除套件快取，並 `nuget locals all -clear`執行命令。</span><span class="sxs-lookup"><span data-stu-id="7151d-1134">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="7151d-1135">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="7151d-1135">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="7151d-1136">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="7151d-1136">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="7151d-1137">重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="7151d-1137">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7151d-1138">其他資源</span><span class="sxs-lookup"><span data-stu-id="7151d-1138">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="7151d-1139">Azure 文件</span><span class="sxs-lookup"><span data-stu-id="7151d-1139">Azure documentation</span></span>

* [<span data-ttu-id="7151d-1140">Application Insights for ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7151d-1140">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="7151d-1141">使用 Visual Studio 在 Azure App Service 中疑難排解 web 應用程式的遠端偵錯程式一節</span><span class="sxs-lookup"><span data-stu-id="7151d-1141">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="7151d-1142">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="7151d-1142">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="7151d-1143">作法：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-1143">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="7151d-1144">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="7151d-1144">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="7151d-1145">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-1145">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="7151d-1146">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7151d-1146">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="7151d-1147">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="7151d-1147">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="7151d-1148">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="7151d-1148">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="7151d-1149">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="7151d-1149">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="7151d-1150">Visual Studio 文件</span><span class="sxs-lookup"><span data-stu-id="7151d-1150">Visual Studio documentation</span></span>

* [<span data-ttu-id="7151d-1151">Visual Studio 2017 中 Azure 上 IIS 的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7151d-1151">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="7151d-1152">Visual Studio 2017 中遠端 IIS 電腦上的遠端 Debug ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7151d-1152">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="7151d-1153">了解使用 Visual Studio 進行偵錯</span><span class="sxs-lookup"><span data-stu-id="7151d-1153">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="7151d-1154">Visual Studio Code 檔</span><span class="sxs-lookup"><span data-stu-id="7151d-1154">Visual Studio Code documentation</span></span>

* <span data-ttu-id="7151d-1155">[使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="7151d-1155">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)</span></span>

::: moniker-end
