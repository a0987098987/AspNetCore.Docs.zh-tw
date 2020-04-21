---
title: 在 Azure 應用服務和 IIS 上 ASP.NET 核心故障
author: rick-anderson
description: 瞭解如何診斷 ASP.NET核心應用的 Azure 應用服務和 Internet 資訊服務 (IIS) 部署的問題。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: f994cd1274bda9082a7cd8b637968b2769db1671
ms.sourcegitcommit: 5547d920f322e5a823575c031529e4755ab119de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81661704"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="18747-103">在 Azure 應用服務和 IIS 上 ASP.NET 核心故障</span><span class="sxs-lookup"><span data-stu-id="18747-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="18747-104">作者：[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="18747-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="18747-105">本文提供有關常見應用啟動錯誤的資訊,以及有關如何在將應用部署到 Azure 應用服務或 IIS 時診斷錯誤的說明:</span><span class="sxs-lookup"><span data-stu-id="18747-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="18747-106">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="18747-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="18747-107">解釋常見的啟動 HTTP 狀態代碼方案。</span><span class="sxs-lookup"><span data-stu-id="18747-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="18747-108">Azure 應用服務上的故障排除</span><span class="sxs-lookup"><span data-stu-id="18747-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="18747-109">為部署到 Azure 應用服務的應用提供故障排除建議。</span><span class="sxs-lookup"><span data-stu-id="18747-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="18747-110">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="18747-111">為部署到IIS或本地在IIS Express上運行的應用提供故障排除建議。</span><span class="sxs-lookup"><span data-stu-id="18747-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="18747-112">本指南適用於 Windows 伺服器和 Windows 桌面部署。</span><span class="sxs-lookup"><span data-stu-id="18747-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="18747-113">清除包快取</span><span class="sxs-lookup"><span data-stu-id="18747-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="18747-114">說明在執行主要升級或更改包版本時,不連貫的包破壞應用時應執行的操作。</span><span class="sxs-lookup"><span data-stu-id="18747-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="18747-115">其他資源</span><span class="sxs-lookup"><span data-stu-id="18747-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="18747-116">列出其他故障排除主題。</span><span class="sxs-lookup"><span data-stu-id="18747-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="18747-117">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="18747-117">App startup errors</span></span>

<span data-ttu-id="18747-118">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="18747-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="18747-119">*502.5 - 進程失敗*或*500.30 -* 本地調試時發生的啟動失敗,可以使用本主題中的建議進行診斷。</span><span class="sxs-lookup"><span data-stu-id="18747-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="18747-120">403.14 禁止</span><span class="sxs-lookup"><span data-stu-id="18747-120">403.14 Forbidden</span></span>

<span data-ttu-id="18747-121">應用無法啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-121">The app fails to start.</span></span> <span data-ttu-id="18747-122">紀錄以下錯誤:</span><span class="sxs-lookup"><span data-stu-id="18747-122">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="18747-123">該錯誤通常是由託管系統上的部署中斷引起的,其中包括以下任何方案:</span><span class="sxs-lookup"><span data-stu-id="18747-123">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="18747-124">該應用程式被部署到託管系統上的錯誤資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-124">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="18747-125">部署過程無法將應用的所有檔案和資料夾移動到託管系統上的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-125">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="18747-126">部署中缺少*Web.config*檔,或者*Web.config*檔內容格式不正確。</span><span class="sxs-lookup"><span data-stu-id="18747-126">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="18747-127">執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="18747-127">Perform the following steps:</span></span>

1. <span data-ttu-id="18747-128">從託管系統上的部署資料夾中刪除所有檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-128">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="18747-129">使用一般部署方法(如 Visual Studio、PowerShell 或手動部署)將應用*發佈*資料夾的內容重新部署到託管系統:</span><span class="sxs-lookup"><span data-stu-id="18747-129">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="18747-130">確認部署中存在*Web.config*檔,並且其內容正確。</span><span class="sxs-lookup"><span data-stu-id="18747-130">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="18747-131">在 Azure 應用服務上託管時,確認應用已部署`D:\home\site\wwwroot`到該資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-131">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="18747-132">當套用用 IIS 託管時,確認應用程式已部署到**IIS 管理員\*\*\*\*的基本設定**中顯示的 IIS**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="18747-132">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="18747-133">通過將託管系統上的部署與專案的*發佈*資料夾的內容進行比較,確認應用的所有檔和資料夾都已部署。</span><span class="sxs-lookup"><span data-stu-id="18747-133">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="18747-134">有關已發布的ASP.NET核心應用佈局的詳細資訊,請參<xref:host-and-deploy/directory-structure>閱 。</span><span class="sxs-lookup"><span data-stu-id="18747-134">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="18747-135">有關*Web.config*檔的詳細資訊,請<xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>參閱 。</span><span class="sxs-lookup"><span data-stu-id="18747-135">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="18747-136">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="18747-136">500 Internal Server Error</span></span>

<span data-ttu-id="18747-137">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="18747-137">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="18747-138">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="18747-138">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="18747-139">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」\*\* 的形式出現。</span><span class="sxs-lookup"><span data-stu-id="18747-139">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="18747-140">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-140">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="18747-141">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="18747-141">From the server's perspective, that's correct.</span></span> <span data-ttu-id="18747-142">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="18747-142">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="18747-143">請在伺服器上於命令提示字元中執行應用程式或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="18747-143">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="18747-144">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="18747-144">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="18747-145">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-145">The worker process fails.</span></span> <span data-ttu-id="18747-146">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-146">The app doesn't start.</span></span>

<span data-ttu-id="18747-147">載入[核心模組元件ASP.NET](xref:host-and-deploy/aspnet-core-module)發生未知錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-147">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="18747-148">請採取下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="18747-148">Take one of the following actions:</span></span>

* <span data-ttu-id="18747-149">連絡 [Microsoft 支援服務](https://support.microsoft.com/oas/default.aspx?prid=15832) (依序選取 [開發人員工具]\*\*\*\* 和 [ASP.NET Core]\*\*\*\*)。</span><span class="sxs-lookup"><span data-stu-id="18747-149">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="18747-150">在 Stack Overflow 上詢問問題。</span><span class="sxs-lookup"><span data-stu-id="18747-150">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="18747-151">在我們的 [GitHub 存放庫](https://github.com/dotnet/AspNetCore)提出問題。</span><span class="sxs-lookup"><span data-stu-id="18747-151">File an issue on our [GitHub repository](https://github.com/dotnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="18747-152">500.30 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="18747-152">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="18747-153">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-153">The worker process fails.</span></span> <span data-ttu-id="18747-154">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-154">The app doesn't start.</span></span>

<span data-ttu-id="18747-155">[ASP.NET核心模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動 .NET 核心 CLR 進程,但未能啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-155">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="18747-156">通常從應用程式事件記錄檔和 ASP.NET Core 模組 stdout 記錄檔中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-156">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="18747-157">常見的故障條件:</span><span class="sxs-lookup"><span data-stu-id="18747-157">Common failure conditions:</span></span>

* <span data-ttu-id="18747-158">由於目標是不存在的ASP.NET核心共用框架的版本,因此應用配置錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-158">The app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="18747-159">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="18747-159">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>
* <span data-ttu-id="18747-160">使用 Azure 金鑰保管庫時,缺少對密鑰保管庫的許可權。</span><span class="sxs-lookup"><span data-stu-id="18747-160">Using Azure Key Vault, lack of permissions to the Key Vault.</span></span> <span data-ttu-id="18747-161">檢查目標密鑰保管庫中的訪問策略,以確保授予正確的許可權。</span><span class="sxs-lookup"><span data-stu-id="18747-161">Check the access policies in the targeted Key Vault to ensure that the correct permissions are granted.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="18747-162">500.31 ANCM 找不到原生相依性</span><span class="sxs-lookup"><span data-stu-id="18747-162">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="18747-163">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-163">The worker process fails.</span></span> <span data-ttu-id="18747-164">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-164">The app doesn't start.</span></span>

<span data-ttu-id="18747-165">[ASP.NET核心模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動 .NET 核心運行時的進程,但它無法啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-165">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="18747-166">此啟動失敗的最常見原因是當 `Microsoft.NETCore.App` 或 `Microsoft.AspNetCore.App` 執行階段未安裝時。</span><span class="sxs-lookup"><span data-stu-id="18747-166">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="18747-167">如果應用程式部署至目標 ASP.NET Core 3.0，但電腦上無該版本，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-167">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="18747-168">範例錯誤訊息如下：</span><span class="sxs-lookup"><span data-stu-id="18747-168">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="18747-169">錯誤訊息會列出所有已安裝 .NET Core 版本和應用程式所要求的版本。</span><span class="sxs-lookup"><span data-stu-id="18747-169">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="18747-170">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="18747-170">To fix this error, either:</span></span>

* <span data-ttu-id="18747-171">在電腦上安裝適當的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="18747-171">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="18747-172">將應用程式的目標 .NET Core 版本變更為電腦上版本。</span><span class="sxs-lookup"><span data-stu-id="18747-172">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="18747-173">將應用程式發佈為[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="18747-173">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="18747-174">在開發過程中執行時 (`ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`)，特定的錯誤會寫入至 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="18747-174">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="18747-175">處理序啟動失敗的原因也會列在應用程式事件記錄檔中。</span><span class="sxs-lookup"><span data-stu-id="18747-175">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="18747-176">500.32 ANCM 無法載入 dll</span><span class="sxs-lookup"><span data-stu-id="18747-176">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="18747-177">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-177">The worker process fails.</span></span> <span data-ttu-id="18747-178">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-178">The app doesn't start.</span></span>

<span data-ttu-id="18747-179">此錯誤最常見原因是針對不相容的處理器架構發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-179">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="18747-180">如果背景工作處理序執行為 32 位元應用程式，而此應用程式已發佈至目標 64 位元，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-180">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="18747-181">若要修正此錯誤，請使用以下其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="18747-181">To fix this error, either:</span></span>

* <span data-ttu-id="18747-182">針對相同的處理器架構，將應用程式重新發佈為背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="18747-182">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="18747-183">將應用程式發佈為[架構相依部署](/dotnet/core/deploying/#framework-dependent-executables-fde)。</span><span class="sxs-lookup"><span data-stu-id="18747-183">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="18747-184">500.33 ANCM 要求處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="18747-184">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="18747-185">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-185">The worker process fails.</span></span> <span data-ttu-id="18747-186">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-186">The app doesn't start.</span></span>

<span data-ttu-id="18747-187">應用程式未參考 `Microsoft.AspNetCore.App` 架構。</span><span class="sxs-lookup"><span data-stu-id="18747-187">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="18747-188">只有面向`Microsoft.AspNetCore.App`框架的應用才能由[ASP.NET核心模組](xref:host-and-deploy/aspnet-core-module)託管。</span><span class="sxs-lookup"><span data-stu-id="18747-188">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="18747-189">若要修正這個錯誤，請確認應用程式以 `Microsoft.AspNetCore.App` 架構為目標。</span><span class="sxs-lookup"><span data-stu-id="18747-189">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="18747-190">檢查 `.runtimeconfig.json` 以驗證應用程式是否以該架構為目標。</span><span class="sxs-lookup"><span data-stu-id="18747-190">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="18747-191">500.34 ANCM 不支援混合式裝載模型</span><span class="sxs-lookup"><span data-stu-id="18747-191">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="18747-192">背景工作處理序無法在相同的程序中執行同處理序應用程式和跨處理序應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-192">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="18747-193">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-193">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="18747-194">500.35 ANCM 同一程序中有多個同處理序應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-194">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="18747-195">工作進程無法在同一進程中運行多個進程內應用。</span><span class="sxs-lookup"><span data-stu-id="18747-195">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="18747-196">若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-196">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="18747-197">500.36 ANCM 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="18747-197">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="18747-198">跨處理序要求處理常式 *aspnetcorev2_outofprocess.dll* 不在 *aspnetcorev2.dll* 檔案旁邊。</span><span class="sxs-lookup"><span data-stu-id="18747-198">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="18747-199">這表示[ASP.NET核心模組](xref:host-and-deploy/aspnet-core-module)的安裝損壞。</span><span class="sxs-lookup"><span data-stu-id="18747-199">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="18747-200">若要修正這個錯誤，請修復 [.NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (適用於 IIS) 或 Visual Studio (適用於 IIS Express) 安裝。</span><span class="sxs-lookup"><span data-stu-id="18747-200">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="18747-201">500.37 ANCM 無法在啟動時間限制內啟動</span><span class="sxs-lookup"><span data-stu-id="18747-201">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="18747-202">ANCM 無法在提供的啟動時間限制內啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-202">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="18747-203">根據預設，逾時值為 120 秒。</span><span class="sxs-lookup"><span data-stu-id="18747-203">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="18747-204">在同一部電腦上啟動大量的應用程式時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-204">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="18747-205">檢查伺服器在啟動期間是否出現 CPU/記憶體的使用量尖峰。</span><span class="sxs-lookup"><span data-stu-id="18747-205">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="18747-206">多個應用程式的啟動程序可能需要交錯進行。</span><span class="sxs-lookup"><span data-stu-id="18747-206">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="50038-ancm-application-dll-not-found"></a><span data-ttu-id="18747-207">500.38 未找到 ANCM 應用程式 DLL</span><span class="sxs-lookup"><span data-stu-id="18747-207">500.38 ANCM Application DLL Not Found</span></span>

<span data-ttu-id="18747-208">ANCM 未能找到應用程式 DLL,該應用程式應位於可執行檔旁邊。</span><span class="sxs-lookup"><span data-stu-id="18747-208">ANCM failed to locate the application DLL, which should be next to the executable.</span></span>

<span data-ttu-id="18747-209">使用進程內託管模型託管打包為[單檔可執行檔](/dotnet/core/whats-new/dotnet-core-3-0#single-file-executables)的應用時,將發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-209">This error occurs when hosting an app packaged as a [single-file executable](/dotnet/core/whats-new/dotnet-core-3-0#single-file-executables) using the in-process hosting model.</span></span> <span data-ttu-id="18747-210">進程內模型要求 ANCM 將 .NET Core 應用載入到現有的 IIS 進程中。</span><span class="sxs-lookup"><span data-stu-id="18747-210">The in-process model requires that the ANCM load the .NET Core app into the existing IIS process.</span></span> <span data-ttu-id="18747-211">單檔部署模型不支援此方案。</span><span class="sxs-lookup"><span data-stu-id="18747-211">This scenario isn't supported by the single-file deployment model.</span></span> <span data-ttu-id="18747-212">使用應用的項目檔中的以下方法**之一**來修復此錯誤:</span><span class="sxs-lookup"><span data-stu-id="18747-212">Use **one** of the following approaches in the app's project file to fix this error:</span></span>

1. <span data-ttu-id="18747-213">通過將`PublishSingleFile`MSBuild 屬性`false`設置為禁用單檔發佈。</span><span class="sxs-lookup"><span data-stu-id="18747-213">Disable single-file publishing by setting the `PublishSingleFile` MSBuild property to `false`.</span></span>
1. <span data-ttu-id="18747-214">通過將`AspNetCoreHostingModel`MSBuild 屬性`OutOfProcess`設置為 ,切換到進程外託管模型。</span><span class="sxs-lookup"><span data-stu-id="18747-214">Switch to the out-of-process hosting model by setting the `AspNetCoreHostingModel` MSBuild property to `OutOfProcess`.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="18747-215">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="18747-215">502.5 Process Failure</span></span>

<span data-ttu-id="18747-216">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-216">The worker process fails.</span></span> <span data-ttu-id="18747-217">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-217">The app doesn't start.</span></span>

<span data-ttu-id="18747-218">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-218">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="18747-219">通常從應用程式事件記錄檔和 ASP.NET Core 模組 stdout 記錄檔中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-219">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="18747-220">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="18747-220">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="18747-221">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="18747-221">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="18747-222">「共用的架構」\*\* 是一組安裝於電腦上且由 `Microsoft.AspNetCore.App` 之類的中繼套件所參考的組件 (*.dll* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="18747-222">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="18747-223">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="18747-223">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="18747-224">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="18747-224">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="18747-225">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗]\*\* 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="18747-225">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="18747-226">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="18747-226">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="18747-227">應用程式無法啟動，因為無法載入應用程式的組件 (*.dll*)。</span><span class="sxs-lookup"><span data-stu-id="18747-227">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="18747-228">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-228">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="18747-229">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="18747-229">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="18747-230">在 IIS 管理員的 [應用程式集區]\*\*\*\* 中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="18747-230">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="18747-231">在 [動作]\*\*\*\* 面板中，選取 [編輯應用程式集區]\*\*\*\* 下的 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-231">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="18747-232">設定 [啟用 32 位元應用程式]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="18747-232">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="18747-233">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="18747-233">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="18747-234">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="18747-234">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="18747-235">確認專案檔中的`<Platform>`MSBuild 屬性與應用的已發佈位之間沒有衝突。</span><span class="sxs-lookup"><span data-stu-id="18747-235">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="18747-236">連線重設</span><span class="sxs-lookup"><span data-stu-id="18747-236">Connection reset</span></span>

<span data-ttu-id="18747-237">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-237">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="18747-238">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-238">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="18747-239">這類錯誤會在用戶端上顯示為「連線重設」\*\* 錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-239">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="18747-240">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="18747-240">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="18747-241">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="18747-241">Default startup limits</span></span>

<span data-ttu-id="18747-242">[ASP.NET核心模組](xref:host-and-deploy/aspnet-core-module)設定了 120 秒的預設*啟動時間限制*。</span><span class="sxs-lookup"><span data-stu-id="18747-242">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="18747-243">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-243">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="18747-244">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="18747-244">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="18747-245">Azure 應用服務上的故障排除</span><span class="sxs-lookup"><span data-stu-id="18747-245">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="18747-246">應用程式事件紀錄(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-246">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="18747-247">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題]\*\*\*\* 刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="18747-247">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="18747-248">在 Azure 入口網站的 [應用程式服務]\*\*\*\* 中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-248">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="18747-249">選取 [診斷並解決問題]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-249">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="18747-250">選取 [診斷工具]\*\*\*\* 標題。</span><span class="sxs-lookup"><span data-stu-id="18747-250">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="18747-251">在 [支援工具]\*\*\*\* 下，選取 [應用程式事件]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-251">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="18747-252">檢查 [來源]\*\*\*\* 資料行中 *IIS AspNetCoreModule* 或 *IIS AspNetCoreModule V2* 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-252">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="18747-253">除了使用 [診斷並解決問題]\*\*\*\* 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="18747-253">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="18747-254">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-254">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="18747-255">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-255">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-256">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-256">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-257">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-257">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-258">開啟 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-258">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="18747-259">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="18747-259">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="18747-260">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-260">Examine the log.</span></span> <span data-ttu-id="18747-261">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="18747-261">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="18747-262">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-262">Run the app in the Kudu console</span></span>

<span data-ttu-id="18747-263">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="18747-263">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="18747-264">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="18747-264">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="18747-265">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-265">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="18747-266">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-266">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-267">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-267">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-268">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-268">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="18747-269">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-269">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="18747-270">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="18747-270">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="18747-271">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="18747-271">Run the app:</span></span>
   * <span data-ttu-id="18747-272">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="18747-272">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="18747-273">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="18747-273">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="18747-274">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-274">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="18747-275">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="18747-275">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="18747-276">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="18747-276">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="18747-277">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="18747-277">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="18747-278">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="18747-278">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="18747-279">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-279">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="18747-280">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-280">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="18747-281">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="18747-281">**Current release**</span></span>

* <span data-ttu-id="18747-282">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="18747-282">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="18747-283">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="18747-283">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="18747-284">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="18747-284">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="18747-285">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="18747-285">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="18747-286">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-286">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="18747-287">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="18747-287">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="18747-288">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="18747-288">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="18747-289">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="18747-289">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="18747-290">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="18747-290">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="18747-291">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-291">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="18747-292">ASP.NET核心模組固定紀錄(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-292">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="18747-293">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="18747-293">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="18747-294">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="18747-294">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="18747-295">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-295">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="18747-296">在 [選取問題類別]\*\*\*\* 底下，選取 [Web 應用程式停止運作]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-296">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="18747-297">在 [建議的解決方案]**[啟用 Stdout 記錄檔重新導向]** > \*\*\*\* 底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-297">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="18747-298">在 Kudu [診斷主控台]\*\*\*\* 中，將資料夾開啟至路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-298">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="18747-299">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-299">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="18747-300">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="18747-300">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="18747-301">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="18747-301">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="18747-302">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-302">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="18747-303">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-303">Make a request to the app.</span></span>
1. <span data-ttu-id="18747-304">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="18747-304">Return to the Azure portal.</span></span> <span data-ttu-id="18747-305">選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-305">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="18747-306">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-306">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-307">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-307">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-308">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-308">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-309">選取 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-309">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="18747-310">檢查 [修改時間]\*\*\*\* 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-310">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="18747-311">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-311">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="18747-312">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-312">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="18747-313">在 Kudu [診斷主控台]\*\*\*\* 中，返回路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\* 以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-313">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="18747-314">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-314">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="18747-315">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="18747-315">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="18747-316">選取 [儲存]\*\*\*\* 以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-316">Select **Save** to save the file.</span></span>

<span data-ttu-id="18747-317">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="18747-317">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-318">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-318">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="18747-319">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="18747-319">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="18747-320">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-320">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="18747-321">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-321">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-322">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-322">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="18747-323">ASP.NET核心模組除錯紀錄(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-323">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="18747-324">ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-324">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="18747-325">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="18747-325">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="18747-326">若要啟用增強型診斷記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="18747-326">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="18747-327">遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-327">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="18747-328">重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-328">Redeploy the app.</span></span>
   * <span data-ttu-id="18747-329">使用 Kudu 主控台，將[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所顯示的 `<handlerSettings>` 新增至即時應用程式的 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="18747-329">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="18747-330">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-330">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="18747-331">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-331">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-332">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-332">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="18747-333">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-333">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="18747-334">打開路徑**站點** > **wwwroot**的資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-334">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="18747-335">選取鉛筆圖示來編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-335">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="18747-336">新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。</span><span class="sxs-lookup"><span data-stu-id="18747-336">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="18747-337">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-337">Select the **Save** button.</span></span>
1. <span data-ttu-id="18747-338">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-338">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="18747-339">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-339">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-340">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-340">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-341">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-341">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-342">打開路徑**站點** > **wwwroot**的資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-342">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="18747-343">如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。</span><span class="sxs-lookup"><span data-stu-id="18747-343">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="18747-344">如果已提供路徑，請巡覽至記錄檔的位置。</span><span class="sxs-lookup"><span data-stu-id="18747-344">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="18747-345">使用檔案名稱旁的鉛筆圖示來開啟記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-345">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="18747-346">完成疑難排解時，請停用偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-346">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="18747-347">若要停用增強型偵錯記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="18747-347">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="18747-348">從 *web.config* 檔案本機移除 `<handlerSettings>` 並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-348">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="18747-349">使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。</span><span class="sxs-lookup"><span data-stu-id="18747-349">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="18747-350">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-350">Save the file.</span></span>

<span data-ttu-id="18747-351">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="18747-351">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-352">無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-352">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="18747-353">記錄檔大小沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="18747-353">There's no limit on log file size.</span></span> <span data-ttu-id="18747-354">請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-354">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="18747-355">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-355">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-356">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-356">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="18747-357">慢速或掛起應用(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-357">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="18747-358">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="18747-358">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="18747-359">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-359">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="18747-360">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="18747-360">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="18747-361">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="18747-361">Monitoring blades</span></span>

<span data-ttu-id="18747-362">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="18747-362">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="18747-363">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-363">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="18747-364">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="18747-364">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="18747-365">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="18747-365">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="18747-366">在 [開發工具]\*\*\*\* 刀鋒視窗區段中，選取 [延伸模組]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-366">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="18747-367">[ASP.NET Core 延伸模組]\*\*\*\* 應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="18747-367">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="18747-368">如果未安裝這些延伸模組，請選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-368">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="18747-369">從清單中選擇 [ASP.NET Core 延伸模組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-369">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="18747-370">選取 [確定]\*\*\*\* 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="18747-370">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="18747-371">在 [新增延伸模組]\*\*\*\* 刀鋒視窗上，選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-371">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="18747-372">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="18747-372">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="18747-373">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="18747-373">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="18747-374">在 Azure 入口網站中，選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-374">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="18747-375">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-375">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-376">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-376">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-377">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-377">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-378">將資料夾開啟至路徑 [site]**[wwwroot]** > \*\*\*\*，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-378">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="18747-379">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="18747-379">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="18747-380">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="18747-380">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="18747-381">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-381">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="18747-382">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-382">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="18747-383">在 Azure 入口網站中，選取 [診斷記錄檔]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-383">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="18747-384">選取 [應用程式記錄 (檔案系統)]\*\*\*\* 和 [詳細錯誤訊息]\*\*\*\*.的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="18747-384">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="18747-385">選取刀鋒視窗頂端的 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-385">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="18747-386">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤]\*\*\*\* 的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="18747-386">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="18747-387">選取 [記錄資料流]\*\*\*\* 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔]\*\*\*\* 刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="18747-387">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="18747-388">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-388">Make a request to the app.</span></span>
1. <span data-ttu-id="18747-389">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-389">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="18747-390">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-390">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="18747-391">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="18747-391">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="18747-392">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-392">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="18747-393">從資訊看板的 [支援工具]\*\*\*\* 區域中，選取 [失敗要求追蹤記錄檔]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-393">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="18747-394">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="18747-394">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="18747-395">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="18747-395">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="18747-396">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-396">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="18747-397">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="18747-397">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="18747-398">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-398">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-399">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-399">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="18747-400">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-400">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="18747-401">應用程式事件紀錄 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-401">Application Event Log (IIS)</span></span>

<span data-ttu-id="18747-402">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="18747-402">Access the Application Event Log:</span></span>

1. <span data-ttu-id="18747-403">打開"開始"功能表,搜索*事件查看器*,然後選擇 **「事件查看器」** 應用。</span><span class="sxs-lookup"><span data-stu-id="18747-403">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="18747-404">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="18747-404">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="18747-405">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="18747-405">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="18747-406">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-406">Search for errors associated with the failing app.</span></span> <span data-ttu-id="18747-407">錯誤在 [來源]\*\* 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="18747-407">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="18747-408">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-408">Run the app at a command prompt</span></span>

<span data-ttu-id="18747-409">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="18747-409">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="18747-410">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-410">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="18747-411">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="18747-411">Framework-dependent deployment</span></span>

<span data-ttu-id="18747-412">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="18747-412">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="18747-413">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-413">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="18747-414">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="18747-414">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="18747-415">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-415">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="18747-416">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-416">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="18747-417">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-417">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="18747-418">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="18747-418">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="18747-419">自封式部署</span><span class="sxs-lookup"><span data-stu-id="18747-419">Self-contained deployment</span></span>

<span data-ttu-id="18747-420">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="18747-420">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="18747-421">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="18747-421">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="18747-422">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="18747-422">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="18747-423">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-423">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="18747-424">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-424">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="18747-425">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-425">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="18747-426">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="18747-426">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="18747-427">ASP.NET核心模組固定紀錄 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-427">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="18747-428">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="18747-428">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="18747-429">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-429">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="18747-430">如果 [logs]\*\* 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-430">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="18747-431">如需有關如何讓 MSBuild 在部署中自動建立 [logs]\*\* 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="18747-431">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="18747-432">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-432">Edit the *web.config* file.</span></span> <span data-ttu-id="18747-433">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs]\*\* 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="18747-433">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="18747-434">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="18747-434">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="18747-435">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="18747-435">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="18747-436">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="18747-436">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="18747-437">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="18747-437">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="18747-438">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-438">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="18747-439">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-439">Make a request to the app.</span></span>
1. <span data-ttu-id="18747-440">瀏覽至 [logs]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-440">Navigate to the *logs* folder.</span></span> <span data-ttu-id="18747-441">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-441">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="18747-442">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-442">Study the log for errors.</span></span>

<span data-ttu-id="18747-443">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-443">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="18747-444">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-444">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="18747-445">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="18747-445">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="18747-446">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-446">Save the file.</span></span>

<span data-ttu-id="18747-447">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="18747-447">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-448">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-448">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="18747-449">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="18747-449">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="18747-450">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-450">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-451">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-451">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="18747-452">ASP.NET核心模組除錯紀錄 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-452">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="18747-453">將以下處理程式設定加入到應用程式的*Web.config*檔,以開啟 ASP.NET 核心模組除錯紀錄:</span><span class="sxs-lookup"><span data-stu-id="18747-453">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="18747-454">確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="18747-454">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="18747-455">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="18747-455">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="18747-456">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="18747-456">Enable the Developer Exception Page</span></span>

<span data-ttu-id="18747-457">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-457">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="18747-458">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="18747-458">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="18747-459">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="18747-459">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="18747-460">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="18747-460">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="18747-461">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="18747-461">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="18747-462">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="18747-462">Obtain data from an app</span></span>

<span data-ttu-id="18747-463">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="18747-463">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="18747-464">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="18747-464">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="18747-465">慢速或暫停應用程式 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-465">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="18747-466">*崩潰轉儲*是系統記憶體的快照,可幫助確定應用崩潰、啟動失敗或應用慢速的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-466">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="18747-467">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="18747-467">App crashes or encounters an exception</span></span>

<span data-ttu-id="18747-468">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="18747-468">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="18747-469">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-469">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="18747-470">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="18747-470">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="18747-471">執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="18747-471">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="18747-472">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-472">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="18747-473">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-473">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="18747-474">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-474">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="18747-475">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="18747-475">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="18747-476">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-476">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="18747-477">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-477">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="18747-478">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-478">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="18747-479">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="18747-479">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-480">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="18747-480">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="18747-481">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="18747-481">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="18747-482">當應用*掛起*(停止回應但不崩潰)、在啟動期間失敗或正常運行時,請參閱[使用者模式轉儲檔:選擇最佳工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)以選擇適當的工具來生成轉儲。</span><span class="sxs-lookup"><span data-stu-id="18747-482">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="18747-483">分析傾印</span><span class="sxs-lookup"><span data-stu-id="18747-483">Analyze the dump</span></span>

<span data-ttu-id="18747-484">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="18747-484">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="18747-485">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="18747-485">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="18747-486">清除包快取</span><span class="sxs-lookup"><span data-stu-id="18747-486">Clear package caches</span></span>

<span data-ttu-id="18747-487">在升級開發電腦上的 .NET Core SDK 或更改應用中的包版本後,正常運行的應用可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-487">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="18747-488">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-488">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="18747-489">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="18747-489">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="18747-490">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-490">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="18747-491">通過執行[dotnet nuget 本地變數](/dotnet/core/tools/dotnet-nuget-locals)來清除包快取 -- 從命令 shell 中清除。</span><span class="sxs-lookup"><span data-stu-id="18747-491">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="18747-492">清除包快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具`nuget locals all -clear`執行命令 。</span><span class="sxs-lookup"><span data-stu-id="18747-492">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="18747-493">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="18747-493">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="18747-494">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="18747-494">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="18747-495">在重新部署應用之前,請刪除伺服器上的部署資料夾中的所有檔。</span><span class="sxs-lookup"><span data-stu-id="18747-495">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="18747-496">其他資源</span><span class="sxs-lookup"><span data-stu-id="18747-496">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="18747-497">Azure 文件</span><span class="sxs-lookup"><span data-stu-id="18747-497">Azure documentation</span></span>

* [<span data-ttu-id="18747-498">ASP.NET Core 的 Application Insights</span><span class="sxs-lookup"><span data-stu-id="18747-498">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="18747-499">使用視覺化工作室在 Azure 應用服務中排除 Web 應用故障的遠端除錯 Web 應用部分</span><span class="sxs-lookup"><span data-stu-id="18747-499">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="18747-500">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="18747-500">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="18747-501">作法：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-501">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="18747-502">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-502">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="18747-503">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-503">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="18747-504">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-504">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="18747-505">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="18747-505">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="18747-506">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="18747-506">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="18747-507">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="18747-507">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="18747-508">Visual Studio 文件</span><span class="sxs-lookup"><span data-stu-id="18747-508">Visual Studio documentation</span></span>

* [<span data-ttu-id="18747-509">遠端除錯ASP.NET視覺化工作室 2017 中 Azure 中的 IIS 核心</span><span class="sxs-lookup"><span data-stu-id="18747-509">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="18747-510">遠端除錯ASP.NET視覺化工作室遠端 IIS 電腦上的核心</span><span class="sxs-lookup"><span data-stu-id="18747-510">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="18747-511">使用視覺化工作室學習除錯</span><span class="sxs-lookup"><span data-stu-id="18747-511">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="18747-512">視覺化工作室代碼文件</span><span class="sxs-lookup"><span data-stu-id="18747-512">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="18747-513">使用 Visual Studio Code 偵錯</span><span class="sxs-lookup"><span data-stu-id="18747-513">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="18747-514">本文提供有關常見應用啟動錯誤的資訊,以及有關如何在將應用部署到 Azure 應用服務或 IIS 時診斷錯誤的說明:</span><span class="sxs-lookup"><span data-stu-id="18747-514">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="18747-515">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="18747-515">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="18747-516">解釋常見的啟動 HTTP 狀態代碼方案。</span><span class="sxs-lookup"><span data-stu-id="18747-516">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="18747-517">Azure 應用服務上的故障排除</span><span class="sxs-lookup"><span data-stu-id="18747-517">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="18747-518">為部署到 Azure 應用服務的應用提供故障排除建議。</span><span class="sxs-lookup"><span data-stu-id="18747-518">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="18747-519">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-519">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="18747-520">為部署到IIS或本地在IIS Express上運行的應用提供故障排除建議。</span><span class="sxs-lookup"><span data-stu-id="18747-520">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="18747-521">本指南適用於 Windows 伺服器和 Windows 桌面部署。</span><span class="sxs-lookup"><span data-stu-id="18747-521">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="18747-522">清除包快取</span><span class="sxs-lookup"><span data-stu-id="18747-522">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="18747-523">說明在執行主要升級或更改包版本時,不連貫的包破壞應用時應執行的操作。</span><span class="sxs-lookup"><span data-stu-id="18747-523">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="18747-524">其他資源</span><span class="sxs-lookup"><span data-stu-id="18747-524">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="18747-525">列出其他故障排除主題。</span><span class="sxs-lookup"><span data-stu-id="18747-525">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="18747-526">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="18747-526">App startup errors</span></span>

<span data-ttu-id="18747-527">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="18747-527">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="18747-528">*502.5 - 進程失敗*或*500.30 -* 本地調試時發生的啟動失敗,可以使用本主題中的建議進行診斷。</span><span class="sxs-lookup"><span data-stu-id="18747-528">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="18747-529">403.14 禁止</span><span class="sxs-lookup"><span data-stu-id="18747-529">403.14 Forbidden</span></span>

<span data-ttu-id="18747-530">應用無法啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-530">The app fails to start.</span></span> <span data-ttu-id="18747-531">紀錄以下錯誤:</span><span class="sxs-lookup"><span data-stu-id="18747-531">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="18747-532">該錯誤通常是由託管系統上的部署中斷引起的,其中包括以下任何方案:</span><span class="sxs-lookup"><span data-stu-id="18747-532">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="18747-533">該應用程式被部署到託管系統上的錯誤資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-533">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="18747-534">部署過程無法將應用的所有檔案和資料夾移動到託管系統上的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-534">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="18747-535">部署中缺少*Web.config*檔,或者*Web.config*檔內容格式不正確。</span><span class="sxs-lookup"><span data-stu-id="18747-535">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="18747-536">執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="18747-536">Perform the following steps:</span></span>

1. <span data-ttu-id="18747-537">從託管系統上的部署資料夾中刪除所有檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-537">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="18747-538">使用一般部署方法(如 Visual Studio、PowerShell 或手動部署)將應用*發佈*資料夾的內容重新部署到託管系統:</span><span class="sxs-lookup"><span data-stu-id="18747-538">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="18747-539">確認部署中存在*Web.config*檔,並且其內容正確。</span><span class="sxs-lookup"><span data-stu-id="18747-539">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="18747-540">在 Azure 應用服務上託管時,確認應用已部署`D:\home\site\wwwroot`到該資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-540">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="18747-541">當套用用 IIS 託管時,確認應用程式已部署到**IIS 管理員\*\*\*\*的基本設定**中顯示的 IIS**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="18747-541">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="18747-542">通過將託管系統上的部署與專案的*發佈*資料夾的內容進行比較,確認應用的所有檔和資料夾都已部署。</span><span class="sxs-lookup"><span data-stu-id="18747-542">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="18747-543">有關已發布的ASP.NET核心應用佈局的詳細資訊,請參<xref:host-and-deploy/directory-structure>閱 。</span><span class="sxs-lookup"><span data-stu-id="18747-543">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="18747-544">有關*Web.config*檔的詳細資訊,請<xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>參閱 。</span><span class="sxs-lookup"><span data-stu-id="18747-544">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="18747-545">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="18747-545">500 Internal Server Error</span></span>

<span data-ttu-id="18747-546">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="18747-546">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="18747-547">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="18747-547">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="18747-548">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」\*\* 的形式出現。</span><span class="sxs-lookup"><span data-stu-id="18747-548">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="18747-549">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-549">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="18747-550">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="18747-550">From the server's perspective, that's correct.</span></span> <span data-ttu-id="18747-551">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="18747-551">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="18747-552">請在伺服器上於命令提示字元中執行應用程式或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="18747-552">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="18747-553">500.0 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="18747-553">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="18747-554">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-554">The worker process fails.</span></span> <span data-ttu-id="18747-555">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-555">The app doesn't start.</span></span>

<span data-ttu-id="18747-556">[ASP.NET核心模組](xref:host-and-deploy/aspnet-core-module)找不到 .NET Core CLR 並查找行程內請求處理程式 *(aspnetcorev2_inprocess.dll*)。</span><span class="sxs-lookup"><span data-stu-id="18747-556">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="18747-557">請檢查︰</span><span class="sxs-lookup"><span data-stu-id="18747-557">Check that:</span></span>

* <span data-ttu-id="18747-558">應用程式以 [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet 套件或 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)為目標。</span><span class="sxs-lookup"><span data-stu-id="18747-558">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="18747-559">應用程式設為目標的 ASP.NET Core 共用架構版本有安裝在目標機器上。</span><span class="sxs-lookup"><span data-stu-id="18747-559">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="18747-560">500.0 跨處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="18747-560">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="18747-561">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-561">The worker process fails.</span></span> <span data-ttu-id="18747-562">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-562">The app doesn't start.</span></span>

<span data-ttu-id="18747-563">[ASP.NET核心模組](xref:host-and-deploy/aspnet-core-module)找不到進程外託管請求處理程式。</span><span class="sxs-lookup"><span data-stu-id="18747-563">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="18747-564">請確定 *aspnetcorev2_outofprocess.dll* 出現在子資料夾中，且位於 *aspnetcorev2.dll* 旁。</span><span class="sxs-lookup"><span data-stu-id="18747-564">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="18747-565">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="18747-565">502.5 Process Failure</span></span>

<span data-ttu-id="18747-566">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-566">The worker process fails.</span></span> <span data-ttu-id="18747-567">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-567">The app doesn't start.</span></span>

<span data-ttu-id="18747-568">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-568">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="18747-569">通常從應用程式事件記錄檔和 ASP.NET Core 模組 stdout 記錄檔中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-569">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="18747-570">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="18747-570">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="18747-571">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="18747-571">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="18747-572">「共用的架構」\*\* 是一組安裝於電腦上且由 `Microsoft.AspNetCore.App` 之類的中繼套件所參考的組件 (*.dll* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="18747-572">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="18747-573">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="18747-573">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="18747-574">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="18747-574">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="18747-575">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗]\*\* 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="18747-575">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="18747-576">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="18747-576">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="18747-577">應用程式無法啟動，因為無法載入應用程式的組件 (*.dll*)。</span><span class="sxs-lookup"><span data-stu-id="18747-577">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="18747-578">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-578">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="18747-579">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="18747-579">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="18747-580">在 IIS 管理員的 [應用程式集區]\*\*\*\* 中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="18747-580">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="18747-581">在 [動作]\*\*\*\* 面板中，選取 [編輯應用程式集區]\*\*\*\* 下的 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-581">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="18747-582">設定 [啟用 32 位元應用程式]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="18747-582">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="18747-583">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="18747-583">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="18747-584">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="18747-584">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="18747-585">確認專案檔中的`<Platform>`MSBuild 屬性與應用的已發佈位之間沒有衝突。</span><span class="sxs-lookup"><span data-stu-id="18747-585">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="18747-586">連線重設</span><span class="sxs-lookup"><span data-stu-id="18747-586">Connection reset</span></span>

<span data-ttu-id="18747-587">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-587">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="18747-588">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-588">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="18747-589">這類錯誤會在用戶端上顯示為「連線重設」\*\* 錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-589">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="18747-590">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="18747-590">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="18747-591">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="18747-591">Default startup limits</span></span>

<span data-ttu-id="18747-592">[ASP.NET核心模組](xref:host-and-deploy/aspnet-core-module)設定了 120 秒的預設*啟動時間限制*。</span><span class="sxs-lookup"><span data-stu-id="18747-592">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="18747-593">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-593">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="18747-594">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="18747-594">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="18747-595">Azure 應用服務上的故障排除</span><span class="sxs-lookup"><span data-stu-id="18747-595">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="18747-596">應用程式事件紀錄(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-596">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="18747-597">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題]\*\*\*\* 刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="18747-597">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="18747-598">在 Azure 入口網站的 [應用程式服務]\*\*\*\* 中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-598">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="18747-599">選取 [診斷並解決問題]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-599">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="18747-600">選取 [診斷工具]\*\*\*\* 標題。</span><span class="sxs-lookup"><span data-stu-id="18747-600">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="18747-601">在 [支援工具]\*\*\*\* 下，選取 [應用程式事件]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-601">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="18747-602">檢查 [來源]\*\*\*\* 資料行中 *IIS AspNetCoreModule* 或 *IIS AspNetCoreModule V2* 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-602">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="18747-603">除了使用 [診斷並解決問題]\*\*\*\* 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="18747-603">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="18747-604">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-604">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="18747-605">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-605">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-606">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-606">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-607">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-607">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-608">開啟 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-608">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="18747-609">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="18747-609">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="18747-610">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-610">Examine the log.</span></span> <span data-ttu-id="18747-611">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="18747-611">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="18747-612">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-612">Run the app in the Kudu console</span></span>

<span data-ttu-id="18747-613">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="18747-613">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="18747-614">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="18747-614">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="18747-615">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-615">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="18747-616">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-616">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-617">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-617">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-618">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-618">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="18747-619">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-619">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="18747-620">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="18747-620">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="18747-621">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="18747-621">Run the app:</span></span>
   * <span data-ttu-id="18747-622">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="18747-622">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="18747-623">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="18747-623">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="18747-624">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-624">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="18747-625">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="18747-625">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="18747-626">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="18747-626">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="18747-627">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="18747-627">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="18747-628">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="18747-628">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="18747-629">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-629">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="18747-630">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-630">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="18747-631">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="18747-631">**Current release**</span></span>

* <span data-ttu-id="18747-632">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="18747-632">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="18747-633">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="18747-633">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="18747-634">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="18747-634">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="18747-635">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="18747-635">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="18747-636">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-636">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="18747-637">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="18747-637">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="18747-638">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="18747-638">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="18747-639">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="18747-639">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="18747-640">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="18747-640">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="18747-641">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-641">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="18747-642">ASP.NET核心模組固定紀錄(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-642">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="18747-643">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="18747-643">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="18747-644">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="18747-644">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="18747-645">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-645">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="18747-646">在 [選取問題類別]\*\*\*\* 底下，選取 [Web 應用程式停止運作]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-646">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="18747-647">在 [建議的解決方案]**[啟用 Stdout 記錄檔重新導向]** > \*\*\*\* 底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-647">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="18747-648">在 Kudu [診斷主控台]\*\*\*\* 中，將資料夾開啟至路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-648">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="18747-649">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-649">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="18747-650">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="18747-650">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="18747-651">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="18747-651">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="18747-652">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-652">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="18747-653">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-653">Make a request to the app.</span></span>
1. <span data-ttu-id="18747-654">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="18747-654">Return to the Azure portal.</span></span> <span data-ttu-id="18747-655">選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-655">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="18747-656">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-656">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-657">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-657">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-658">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-658">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-659">選取 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-659">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="18747-660">檢查 [修改時間]\*\*\*\* 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-660">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="18747-661">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-661">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="18747-662">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-662">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="18747-663">在 Kudu [診斷主控台]\*\*\*\* 中，返回路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\* 以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-663">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="18747-664">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-664">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="18747-665">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="18747-665">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="18747-666">選取 [儲存]\*\*\*\* 以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-666">Select **Save** to save the file.</span></span>

<span data-ttu-id="18747-667">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="18747-667">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-668">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-668">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="18747-669">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="18747-669">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="18747-670">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-670">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="18747-671">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-671">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-672">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-672">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="18747-673">ASP.NET核心模組除錯紀錄(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-673">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="18747-674">ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-674">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="18747-675">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="18747-675">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="18747-676">若要啟用增強型診斷記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="18747-676">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="18747-677">遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-677">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="18747-678">重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-678">Redeploy the app.</span></span>
   * <span data-ttu-id="18747-679">使用 Kudu 主控台，將[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所顯示的 `<handlerSettings>` 新增至即時應用程式的 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="18747-679">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="18747-680">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-680">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="18747-681">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-681">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-682">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-682">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="18747-683">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-683">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="18747-684">打開路徑**站點** > **wwwroot**的資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-684">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="18747-685">選取鉛筆圖示來編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-685">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="18747-686">新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。</span><span class="sxs-lookup"><span data-stu-id="18747-686">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="18747-687">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-687">Select the **Save** button.</span></span>
1. <span data-ttu-id="18747-688">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-688">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="18747-689">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-689">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-690">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-690">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-691">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-691">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-692">打開路徑**站點** > **wwwroot**的資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-692">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="18747-693">如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。</span><span class="sxs-lookup"><span data-stu-id="18747-693">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="18747-694">如果已提供路徑，請巡覽至記錄檔的位置。</span><span class="sxs-lookup"><span data-stu-id="18747-694">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="18747-695">使用檔案名稱旁的鉛筆圖示來開啟記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-695">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="18747-696">完成疑難排解時，請停用偵錯記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-696">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="18747-697">若要停用增強型偵錯記錄，請執行下列任一動作：</span><span class="sxs-lookup"><span data-stu-id="18747-697">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="18747-698">從 *web.config* 檔案本機移除 `<handlerSettings>` 並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-698">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="18747-699">使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。</span><span class="sxs-lookup"><span data-stu-id="18747-699">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="18747-700">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-700">Save the file.</span></span>

<span data-ttu-id="18747-701">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="18747-701">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-702">無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-702">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="18747-703">記錄檔大小沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="18747-703">There's no limit on log file size.</span></span> <span data-ttu-id="18747-704">請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-704">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="18747-705">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-705">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-706">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-706">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="18747-707">慢速或掛起應用(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-707">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="18747-708">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="18747-708">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="18747-709">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-709">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="18747-710">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="18747-710">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="18747-711">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="18747-711">Monitoring blades</span></span>

<span data-ttu-id="18747-712">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="18747-712">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="18747-713">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-713">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="18747-714">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="18747-714">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="18747-715">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="18747-715">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="18747-716">在 [開發工具]\*\*\*\* 刀鋒視窗區段中，選取 [延伸模組]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-716">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="18747-717">[ASP.NET Core 延伸模組]\*\*\*\* 應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="18747-717">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="18747-718">如果未安裝這些延伸模組，請選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-718">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="18747-719">從清單中選擇 [ASP.NET Core 延伸模組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-719">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="18747-720">選取 [確定]\*\*\*\* 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="18747-720">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="18747-721">在 [新增延伸模組]\*\*\*\* 刀鋒視窗上，選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-721">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="18747-722">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="18747-722">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="18747-723">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="18747-723">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="18747-724">在 Azure 入口網站中，選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-724">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="18747-725">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-725">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-726">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-726">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-727">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-727">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-728">將資料夾開啟至路徑 [site]**[wwwroot]** > \*\*\*\*，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-728">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="18747-729">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="18747-729">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="18747-730">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="18747-730">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="18747-731">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-731">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="18747-732">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-732">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="18747-733">在 Azure 入口網站中，選取 [診斷記錄檔]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-733">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="18747-734">選取 [應用程式記錄 (檔案系統)]\*\*\*\* 和 [詳細錯誤訊息]\*\*\*\*.的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="18747-734">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="18747-735">選取刀鋒視窗頂端的 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-735">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="18747-736">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤]\*\*\*\* 的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="18747-736">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="18747-737">選取 [記錄資料流]\*\*\*\* 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔]\*\*\*\* 刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="18747-737">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="18747-738">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-738">Make a request to the app.</span></span>
1. <span data-ttu-id="18747-739">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-739">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="18747-740">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-740">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="18747-741">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="18747-741">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="18747-742">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-742">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="18747-743">從資訊看板的 [支援工具]\*\*\*\* 區域中，選取 [失敗要求追蹤記錄檔]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-743">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="18747-744">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="18747-744">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="18747-745">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="18747-745">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="18747-746">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-746">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="18747-747">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="18747-747">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="18747-748">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-748">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-749">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-749">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="18747-750">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-750">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="18747-751">應用程式事件紀錄 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-751">Application Event Log (IIS)</span></span>

<span data-ttu-id="18747-752">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="18747-752">Access the Application Event Log:</span></span>

1. <span data-ttu-id="18747-753">打開"開始"功能表,搜索*事件查看器*,然後選擇 **「事件查看器」** 應用。</span><span class="sxs-lookup"><span data-stu-id="18747-753">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="18747-754">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="18747-754">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="18747-755">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="18747-755">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="18747-756">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-756">Search for errors associated with the failing app.</span></span> <span data-ttu-id="18747-757">錯誤在 [來源]\*\* 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="18747-757">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="18747-758">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-758">Run the app at a command prompt</span></span>

<span data-ttu-id="18747-759">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="18747-759">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="18747-760">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-760">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="18747-761">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="18747-761">Framework-dependent deployment</span></span>

<span data-ttu-id="18747-762">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="18747-762">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="18747-763">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-763">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="18747-764">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="18747-764">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="18747-765">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-765">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="18747-766">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-766">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="18747-767">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-767">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="18747-768">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="18747-768">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="18747-769">自封式部署</span><span class="sxs-lookup"><span data-stu-id="18747-769">Self-contained deployment</span></span>

<span data-ttu-id="18747-770">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="18747-770">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="18747-771">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="18747-771">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="18747-772">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="18747-772">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="18747-773">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-773">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="18747-774">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-774">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="18747-775">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-775">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="18747-776">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="18747-776">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="18747-777">ASP.NET核心模組固定紀錄 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-777">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="18747-778">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="18747-778">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="18747-779">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-779">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="18747-780">如果 [logs]\*\* 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-780">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="18747-781">如需有關如何讓 MSBuild 在部署中自動建立 [logs]\*\* 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="18747-781">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="18747-782">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-782">Edit the *web.config* file.</span></span> <span data-ttu-id="18747-783">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs]\*\* 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="18747-783">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="18747-784">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="18747-784">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="18747-785">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="18747-785">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="18747-786">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="18747-786">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="18747-787">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="18747-787">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="18747-788">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-788">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="18747-789">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-789">Make a request to the app.</span></span>
1. <span data-ttu-id="18747-790">瀏覽至 [logs]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-790">Navigate to the *logs* folder.</span></span> <span data-ttu-id="18747-791">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-791">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="18747-792">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-792">Study the log for errors.</span></span>

<span data-ttu-id="18747-793">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-793">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="18747-794">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-794">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="18747-795">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="18747-795">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="18747-796">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-796">Save the file.</span></span>

<span data-ttu-id="18747-797">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="18747-797">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-798">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-798">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="18747-799">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="18747-799">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="18747-800">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-800">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-801">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-801">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="18747-802">ASP.NET核心模組除錯紀錄 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-802">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="18747-803">將以下處理程式設定加入到應用程式的*Web.config*檔,以開啟 ASP.NET 核心模組除錯紀錄:</span><span class="sxs-lookup"><span data-stu-id="18747-803">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="18747-804">確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="18747-804">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="18747-805">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。</span><span class="sxs-lookup"><span data-stu-id="18747-805">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="18747-806">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="18747-806">Enable the Developer Exception Page</span></span>

<span data-ttu-id="18747-807">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-807">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="18747-808">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="18747-808">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="18747-809">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="18747-809">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="18747-810">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="18747-810">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="18747-811">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="18747-811">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="18747-812">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="18747-812">Obtain data from an app</span></span>

<span data-ttu-id="18747-813">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="18747-813">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="18747-814">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="18747-814">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="18747-815">慢速或暫停應用程式 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-815">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="18747-816">*崩潰轉儲*是系統記憶體的快照,可幫助確定應用崩潰、啟動失敗或應用慢速的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-816">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="18747-817">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="18747-817">App crashes or encounters an exception</span></span>

<span data-ttu-id="18747-818">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="18747-818">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="18747-819">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-819">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="18747-820">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="18747-820">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="18747-821">執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="18747-821">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="18747-822">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-822">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="18747-823">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-823">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="18747-824">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-824">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="18747-825">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="18747-825">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="18747-826">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-826">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="18747-827">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-827">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="18747-828">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-828">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="18747-829">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="18747-829">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-830">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="18747-830">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="18747-831">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="18747-831">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="18747-832">當應用*掛起*(停止回應但不崩潰)、在啟動期間失敗或正常運行時,請參閱[使用者模式轉儲檔:選擇最佳工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)以選擇適當的工具來生成轉儲。</span><span class="sxs-lookup"><span data-stu-id="18747-832">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="18747-833">分析傾印</span><span class="sxs-lookup"><span data-stu-id="18747-833">Analyze the dump</span></span>

<span data-ttu-id="18747-834">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="18747-834">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="18747-835">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="18747-835">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="18747-836">清除包快取</span><span class="sxs-lookup"><span data-stu-id="18747-836">Clear package caches</span></span>

<span data-ttu-id="18747-837">在升級開發電腦上的 .NET Core SDK 或更改應用中的包版本後,正常運行的應用可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-837">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="18747-838">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-838">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="18747-839">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="18747-839">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="18747-840">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-840">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="18747-841">通過執行[dotnet nuget 本地變數](/dotnet/core/tools/dotnet-nuget-locals)來清除包快取 -- 從命令 shell 中清除。</span><span class="sxs-lookup"><span data-stu-id="18747-841">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="18747-842">清除包快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具`nuget locals all -clear`執行命令 。</span><span class="sxs-lookup"><span data-stu-id="18747-842">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="18747-843">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="18747-843">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="18747-844">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="18747-844">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="18747-845">在重新部署應用之前,請刪除伺服器上的部署資料夾中的所有檔。</span><span class="sxs-lookup"><span data-stu-id="18747-845">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="18747-846">其他資源</span><span class="sxs-lookup"><span data-stu-id="18747-846">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="18747-847">Azure 文件</span><span class="sxs-lookup"><span data-stu-id="18747-847">Azure documentation</span></span>

* [<span data-ttu-id="18747-848">ASP.NET Core 的 Application Insights</span><span class="sxs-lookup"><span data-stu-id="18747-848">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="18747-849">使用視覺化工作室在 Azure 應用服務中排除 Web 應用故障的遠端除錯 Web 應用部分</span><span class="sxs-lookup"><span data-stu-id="18747-849">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="18747-850">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="18747-850">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="18747-851">作法：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-851">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="18747-852">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-852">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="18747-853">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-853">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="18747-854">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-854">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="18747-855">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="18747-855">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="18747-856">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="18747-856">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="18747-857">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="18747-857">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="18747-858">Visual Studio 文件</span><span class="sxs-lookup"><span data-stu-id="18747-858">Visual Studio documentation</span></span>

* [<span data-ttu-id="18747-859">遠端除錯ASP.NET視覺化工作室 2017 中 Azure 中的 IIS 核心</span><span class="sxs-lookup"><span data-stu-id="18747-859">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="18747-860">遠端除錯ASP.NET視覺化工作室遠端 IIS 電腦上的核心</span><span class="sxs-lookup"><span data-stu-id="18747-860">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="18747-861">使用視覺化工作室學習除錯</span><span class="sxs-lookup"><span data-stu-id="18747-861">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="18747-862">視覺化工作室代碼文件</span><span class="sxs-lookup"><span data-stu-id="18747-862">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="18747-863">使用 Visual Studio Code 偵錯</span><span class="sxs-lookup"><span data-stu-id="18747-863">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="18747-864">本文提供有關常見應用啟動錯誤的資訊,以及有關如何在將應用部署到 Azure 應用服務或 IIS 時診斷錯誤的說明:</span><span class="sxs-lookup"><span data-stu-id="18747-864">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="18747-865">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="18747-865">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="18747-866">解釋常見的啟動 HTTP 狀態代碼方案。</span><span class="sxs-lookup"><span data-stu-id="18747-866">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="18747-867">Azure 應用服務上的故障排除</span><span class="sxs-lookup"><span data-stu-id="18747-867">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="18747-868">為部署到 Azure 應用服務的應用提供故障排除建議。</span><span class="sxs-lookup"><span data-stu-id="18747-868">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="18747-869">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-869">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="18747-870">為部署到IIS或本地在IIS Express上運行的應用提供故障排除建議。</span><span class="sxs-lookup"><span data-stu-id="18747-870">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="18747-871">本指南適用於 Windows 伺服器和 Windows 桌面部署。</span><span class="sxs-lookup"><span data-stu-id="18747-871">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="18747-872">清除包快取</span><span class="sxs-lookup"><span data-stu-id="18747-872">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="18747-873">說明在執行主要升級或更改包版本時,不連貫的包破壞應用時應執行的操作。</span><span class="sxs-lookup"><span data-stu-id="18747-873">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="18747-874">其他資源</span><span class="sxs-lookup"><span data-stu-id="18747-874">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="18747-875">列出其他故障排除主題。</span><span class="sxs-lookup"><span data-stu-id="18747-875">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="18747-876">應用程式啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="18747-876">App startup errors</span></span>

<span data-ttu-id="18747-877">在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。</span><span class="sxs-lookup"><span data-stu-id="18747-877">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="18747-878">可以使用本主題中的建議診斷本地除錯時發生的*502.5 行程失敗*。</span><span class="sxs-lookup"><span data-stu-id="18747-878">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="18747-879">403.14 禁止</span><span class="sxs-lookup"><span data-stu-id="18747-879">403.14 Forbidden</span></span>

<span data-ttu-id="18747-880">應用無法啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-880">The app fails to start.</span></span> <span data-ttu-id="18747-881">紀錄以下錯誤:</span><span class="sxs-lookup"><span data-stu-id="18747-881">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="18747-882">該錯誤通常是由託管系統上的部署中斷引起的,其中包括以下任何方案:</span><span class="sxs-lookup"><span data-stu-id="18747-882">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="18747-883">該應用程式被部署到託管系統上的錯誤資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-883">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="18747-884">部署過程無法將應用的所有檔案和資料夾移動到託管系統上的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-884">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="18747-885">部署中缺少*Web.config*檔,或者*Web.config*檔內容格式不正確。</span><span class="sxs-lookup"><span data-stu-id="18747-885">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="18747-886">執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="18747-886">Perform the following steps:</span></span>

1. <span data-ttu-id="18747-887">從託管系統上的部署資料夾中刪除所有檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-887">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="18747-888">使用一般部署方法(如 Visual Studio、PowerShell 或手動部署)將應用*發佈*資料夾的內容重新部署到託管系統:</span><span class="sxs-lookup"><span data-stu-id="18747-888">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="18747-889">確認部署中存在*Web.config*檔,並且其內容正確。</span><span class="sxs-lookup"><span data-stu-id="18747-889">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="18747-890">在 Azure 應用服務上託管時,確認應用已部署`D:\home\site\wwwroot`到該資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-890">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="18747-891">當套用用 IIS 託管時,確認應用程式已部署到**IIS 管理員\*\*\*\*的基本設定**中顯示的 IIS**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="18747-891">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="18747-892">通過將託管系統上的部署與專案的*發佈*資料夾的內容進行比較,確認應用的所有檔和資料夾都已部署。</span><span class="sxs-lookup"><span data-stu-id="18747-892">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="18747-893">有關已發布的ASP.NET核心應用佈局的詳細資訊,請參<xref:host-and-deploy/directory-structure>閱 。</span><span class="sxs-lookup"><span data-stu-id="18747-893">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="18747-894">有關*Web.config*檔的詳細資訊,請<xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>參閱 。</span><span class="sxs-lookup"><span data-stu-id="18747-894">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="18747-895">500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="18747-895">500 Internal Server Error</span></span>

<span data-ttu-id="18747-896">應用程式啟動，但有錯誤導致伺服器無法完成要求。</span><span class="sxs-lookup"><span data-stu-id="18747-896">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="18747-897">此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。</span><span class="sxs-lookup"><span data-stu-id="18747-897">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="18747-898">回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」\*\* 的形式出現。</span><span class="sxs-lookup"><span data-stu-id="18747-898">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="18747-899">「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-899">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="18747-900">從伺服器的觀點來看，這是正確的。</span><span class="sxs-lookup"><span data-stu-id="18747-900">From the server's perspective, that's correct.</span></span> <span data-ttu-id="18747-901">應用程式已啟動，但無法產生有效的回應。</span><span class="sxs-lookup"><span data-stu-id="18747-901">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="18747-902">請在伺服器上於命令提示字元中執行應用程式或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="18747-902">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="18747-903">502.5 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="18747-903">502.5 Process Failure</span></span>

<span data-ttu-id="18747-904">背景工作處理序失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-904">The worker process fails.</span></span> <span data-ttu-id="18747-905">應用程式未啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-905">The app doesn't start.</span></span>

<span data-ttu-id="18747-906">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-906">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="18747-907">通常從應用程式事件記錄檔和 ASP.NET Core 模組 stdout 記錄檔中的項目，即可判斷啟動失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-907">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="18747-908">因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。</span><span class="sxs-lookup"><span data-stu-id="18747-908">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="18747-909">請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。</span><span class="sxs-lookup"><span data-stu-id="18747-909">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="18747-910">「共用的架構」\*\* 是一組安裝於電腦上且由 `Microsoft.AspNetCore.App` 之類的中繼套件所參考的組件 (*.dll* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="18747-910">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="18747-911">中繼套件參考可以指定最低必要版本。</span><span class="sxs-lookup"><span data-stu-id="18747-911">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="18747-912">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="18747-912">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="18747-913">當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗]\*\* 錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="18747-913">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="18747-914">無法啟動應用程式 (ErrorCode '0x800700c1')</span><span class="sxs-lookup"><span data-stu-id="18747-914">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="18747-915">應用程式無法啟動，因為無法載入應用程式的組件 (*.dll*)。</span><span class="sxs-lookup"><span data-stu-id="18747-915">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="18747-916">當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-916">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="18747-917">確認應用程式集區的 32 位元設定正確：</span><span class="sxs-lookup"><span data-stu-id="18747-917">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="18747-918">在 IIS 管理員的 [應用程式集區]\*\*\*\* 中，選取應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="18747-918">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="18747-919">在 [動作]\*\*\*\* 面板中，選取 [編輯應用程式集區]\*\*\*\* 下的 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-919">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="18747-920">設定 [啟用 32 位元應用程式]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="18747-920">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="18747-921">如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。</span><span class="sxs-lookup"><span data-stu-id="18747-921">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="18747-922">如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="18747-922">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="18747-923">確認專案檔中的`<Platform>`MSBuild 屬性與應用的已發佈位之間沒有衝突。</span><span class="sxs-lookup"><span data-stu-id="18747-923">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="18747-924">連線重設</span><span class="sxs-lookup"><span data-stu-id="18747-924">Connection reset</span></span>

<span data-ttu-id="18747-925">如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-925">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="18747-926">通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-926">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="18747-927">這類錯誤會在用戶端上顯示為「連線重設」\*\* 錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-927">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="18747-928">[應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="18747-928">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="18747-929">預設啟動限制</span><span class="sxs-lookup"><span data-stu-id="18747-929">Default startup limits</span></span>

<span data-ttu-id="18747-930">[ASP.NET核心模組](xref:host-and-deploy/aspnet-core-module)設定了 120 秒的預設*啟動時間限制*。</span><span class="sxs-lookup"><span data-stu-id="18747-930">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="18747-931">保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。</span><span class="sxs-lookup"><span data-stu-id="18747-931">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="18747-932">如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="18747-932">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="18747-933">Azure 應用服務上的故障排除</span><span class="sxs-lookup"><span data-stu-id="18747-933">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="18747-934">應用程式事件紀錄(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-934">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="18747-935">若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題]\*\*\*\* 刀鋒視窗：</span><span class="sxs-lookup"><span data-stu-id="18747-935">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="18747-936">在 Azure 入口網站的 [應用程式服務]\*\*\*\* 中，開啟應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-936">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="18747-937">選取 [診斷並解決問題]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-937">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="18747-938">選取 [診斷工具]\*\*\*\* 標題。</span><span class="sxs-lookup"><span data-stu-id="18747-938">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="18747-939">在 [支援工具]\*\*\*\* 下，選取 [應用程式事件]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-939">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="18747-940">檢查 [來源]\*\*\*\* 資料行中 *IIS AspNetCoreModule* 或 *IIS AspNetCoreModule V2* 項目所提供的最新錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-940">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="18747-941">除了使用 [診斷並解決問題]\*\*\*\* 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="18747-941">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="18747-942">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-942">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="18747-943">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-943">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-944">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-944">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-945">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-945">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-946">開啟 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-946">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="18747-947">選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="18747-947">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="18747-948">檢查記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-948">Examine the log.</span></span> <span data-ttu-id="18747-949">捲動至記錄檔的底部以查看最新事件。</span><span class="sxs-lookup"><span data-stu-id="18747-949">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="18747-950">在 Kudu 主控台中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-950">Run the app in the Kudu console</span></span>

<span data-ttu-id="18747-951">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="18747-951">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="18747-952">您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：</span><span class="sxs-lookup"><span data-stu-id="18747-952">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="18747-953">在 [開發工具]\*\*\*\* 區域中，開啟 [進階工具]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-953">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="18747-954">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-954">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-955">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-955">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-956">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-956">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="18747-957">測試 32 位元 (x86) 應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-957">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="18747-958">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="18747-958">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="18747-959">執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="18747-959">Run the app:</span></span>
   * <span data-ttu-id="18747-960">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="18747-960">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="18747-961">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="18747-961">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="18747-962">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-962">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="18747-963">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="18747-963">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="18747-964">*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="18747-964">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="18747-965">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="18747-965">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="18747-966">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="18747-966">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="18747-967">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-967">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="18747-968">測試 64 位元 (x64) 應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-968">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="18747-969">**目前的版本**</span><span class="sxs-lookup"><span data-stu-id="18747-969">**Current release**</span></span>

* <span data-ttu-id="18747-970">如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="18747-970">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="18747-971">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="18747-971">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="18747-972">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="18747-972">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="18747-973">執行應用程式：`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="18747-973">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="18747-974">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-974">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="18747-975">**在預覽版上執行的架構相依部署**</span><span class="sxs-lookup"><span data-stu-id="18747-975">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="18747-976">*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*</span><span class="sxs-lookup"><span data-stu-id="18747-976">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="18747-977">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)</span><span class="sxs-lookup"><span data-stu-id="18747-977">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="18747-978">執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="18747-978">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="18747-979">來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。</span><span class="sxs-lookup"><span data-stu-id="18747-979">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="18747-980">ASP.NET核心模組固定紀錄(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-980">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="18747-981">ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。</span><span class="sxs-lookup"><span data-stu-id="18747-981">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="18747-982">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="18747-982">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="18747-983">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-983">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="18747-984">在 [選取問題類別]\*\*\*\* 底下，選取 [Web 應用程式停止運作]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-984">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="18747-985">在 [建議的解決方案]**[啟用 Stdout 記錄檔重新導向]** > \*\*\*\* 底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-985">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="18747-986">在 Kudu [診斷主控台]\*\*\*\* 中，將資料夾開啟至路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-986">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="18747-987">向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-987">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="18747-988">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="18747-988">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="18747-989">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="18747-989">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="18747-990">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-990">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="18747-991">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-991">Make a request to the app.</span></span>
1. <span data-ttu-id="18747-992">返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="18747-992">Return to the Azure portal.</span></span> <span data-ttu-id="18747-993">選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-993">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="18747-994">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-994">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-995">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-995">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-996">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-996">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-997">選取 **LogFiles** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-997">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="18747-998">檢查 [修改時間]\*\*\*\* 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-998">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="18747-999">當記錄檔開啟時，會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-999">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="18747-1000">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-1000">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="18747-1001">在 Kudu [診斷主控台]\*\*\*\* 中，返回路徑 [site]\*\*\*\* > [wwwroot]\*\*\*\* 以顯露 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-1001">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="18747-1002">再次選取鉛筆圖示來開啟 **web.config** 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-1002">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="18747-1003">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="18747-1003">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="18747-1004">選取 [儲存]\*\*\*\* 以儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-1004">Select **Save** to save the file.</span></span>

<span data-ttu-id="18747-1005">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="18747-1005">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-1006">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-1006">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="18747-1007">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="18747-1007">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="18747-1008">請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-1008">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="18747-1009">針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-1009">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-1010">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-1010">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="18747-1011">慢速或掛起應用(Azure 應用服務)</span><span class="sxs-lookup"><span data-stu-id="18747-1011">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="18747-1012">當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="18747-1012">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="18747-1013">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-1013">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* <span data-ttu-id="18747-1014">[使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="18747-1014">[Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)</span></span>

### <a name="monitoring-blades"></a><span data-ttu-id="18747-1015">監視刀鋒視窗</span><span class="sxs-lookup"><span data-stu-id="18747-1015">Monitoring blades</span></span>

<span data-ttu-id="18747-1016">監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。</span><span class="sxs-lookup"><span data-stu-id="18747-1016">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="18747-1017">這些刀鋒視窗可用來診斷 500 系列的錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-1017">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="18747-1018">請確認已安裝 ASP.NET Core 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="18747-1018">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="18747-1019">如果未安裝這些延伸模組，請手動安裝：</span><span class="sxs-lookup"><span data-stu-id="18747-1019">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="18747-1020">在 [開發工具]\*\*\*\* 刀鋒視窗區段中，選取 [延伸模組]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-1020">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="18747-1021">[ASP.NET Core 延伸模組]\*\*\*\* 應該會出現在清單中。</span><span class="sxs-lookup"><span data-stu-id="18747-1021">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="18747-1022">如果未安裝這些延伸模組，請選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-1022">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="18747-1023">從清單中選擇 [ASP.NET Core 延伸模組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-1023">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="18747-1024">選取 [確定]\*\*\*\* 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="18747-1024">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="18747-1025">在 [新增延伸模組]\*\*\*\* 刀鋒視窗上，選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-1025">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="18747-1026">成功安裝延伸模組時，會有資訊快顯訊息提供指示。</span><span class="sxs-lookup"><span data-stu-id="18747-1026">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="18747-1027">如果未啟用 stdout 記錄，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="18747-1027">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="18747-1028">在 Azure 入口網站中，選取 [開發工具]\*\*\*\* 區域中的 [進階工具]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-1028">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="18747-1029">選取 [執行&rarr;]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-1029">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="18747-1030">Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="18747-1030">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="18747-1031">使用頁面頂端的導覽列，開啟 [偵錯主控台]\*\*\*\*，然後選取 [CMD]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-1031">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="18747-1032">將資料夾開啟至路徑 [site]**[wwwroot]** > \*\*\*\*，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-1032">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="18747-1033">按一下 *web.config* 檔案旁邊的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="18747-1033">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="18747-1034">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="18747-1034">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="18747-1035">選取 [儲存]\*\*\*\* 以儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-1035">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="18747-1036">繼續接著啟用診斷記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-1036">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="18747-1037">在 Azure 入口網站中，選取 [診斷記錄檔]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-1037">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="18747-1038">選取 [應用程式記錄 (檔案系統)]\*\*\*\* 和 [詳細錯誤訊息]\*\*\*\*.的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="18747-1038">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="18747-1039">選取刀鋒視窗頂端的 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="18747-1039">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="18747-1040">若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤]\*\*\*\* 的 [開啟]\*\*\*\* 開關。</span><span class="sxs-lookup"><span data-stu-id="18747-1040">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="18747-1041">選取 [記錄資料流]\*\*\*\* 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔]\*\*\*\* 刀鋒視窗之下)。</span><span class="sxs-lookup"><span data-stu-id="18747-1041">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="18747-1042">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-1042">Make a request to the app.</span></span>
1. <span data-ttu-id="18747-1043">在記錄資料流資料內，會指出錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-1043">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="18747-1044">完成疑難排解時，請務必停用 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="18747-1044">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="18747-1045">檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：</span><span class="sxs-lookup"><span data-stu-id="18747-1045">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="18747-1046">在 Azure 入口網站中，瀏覽至 [診斷並解決問題]\*\*\*\* 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-1046">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="18747-1047">從資訊看板的 [支援工具]\*\*\*\* 區域中，選取 [失敗要求追蹤記錄檔]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="18747-1047">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="18747-1048">如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。</span><span class="sxs-lookup"><span data-stu-id="18747-1048">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="18747-1049">如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。</span><span class="sxs-lookup"><span data-stu-id="18747-1049">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="18747-1050">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-1050">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="18747-1051">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="18747-1051">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="18747-1052">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-1052">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-1053">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-1053">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="18747-1054">針對 IIS 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-1054">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="18747-1055">應用程式事件紀錄 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-1055">Application Event Log (IIS)</span></span>

<span data-ttu-id="18747-1056">存取「應用程式事件記錄檔」：</span><span class="sxs-lookup"><span data-stu-id="18747-1056">Access the Application Event Log:</span></span>

1. <span data-ttu-id="18747-1057">打開"開始"功能表,搜索*事件查看器*,然後選擇 **「事件查看器」** 應用。</span><span class="sxs-lookup"><span data-stu-id="18747-1057">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="18747-1058">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="18747-1058">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="18747-1059">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="18747-1059">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="18747-1060">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-1060">Search for errors associated with the failing app.</span></span> <span data-ttu-id="18747-1061">錯誤在 [來源]\*\* 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。</span><span class="sxs-lookup"><span data-stu-id="18747-1061">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="18747-1062">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-1062">Run the app at a command prompt</span></span>

<span data-ttu-id="18747-1063">許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。</span><span class="sxs-lookup"><span data-stu-id="18747-1063">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="18747-1064">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-1064">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="18747-1065">與 Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="18747-1065">Framework-dependent deployment</span></span>

<span data-ttu-id="18747-1066">如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="18747-1066">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="18747-1067">在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-1067">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="18747-1068">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。</span><span class="sxs-lookup"><span data-stu-id="18747-1068">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="18747-1069">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-1069">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="18747-1070">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-1070">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="18747-1071">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-1071">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="18747-1072">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="18747-1072">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="18747-1073">自封式部署</span><span class="sxs-lookup"><span data-stu-id="18747-1073">Self-contained deployment</span></span>

<span data-ttu-id="18747-1074">如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：</span><span class="sxs-lookup"><span data-stu-id="18747-1074">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="18747-1075">在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="18747-1075">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="18747-1076">在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。</span><span class="sxs-lookup"><span data-stu-id="18747-1076">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="18747-1077">來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="18747-1077">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="18747-1078">如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-1078">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="18747-1079">如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-1079">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="18747-1080">如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。</span><span class="sxs-lookup"><span data-stu-id="18747-1080">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="18747-1081">ASP.NET核心模組固定紀錄 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-1081">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="18747-1082">啟用及檢視 stdout 記錄檔：</span><span class="sxs-lookup"><span data-stu-id="18747-1082">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="18747-1083">瀏覽至主控系統上網站的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-1083">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="18747-1084">如果 [logs]\*\* 資料夾不存在，請建立該資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-1084">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="18747-1085">如需有關如何讓 MSBuild 在部署中自動建立 [logs]\*\* 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="18747-1085">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="18747-1086">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-1086">Edit the *web.config* file.</span></span> <span data-ttu-id="18747-1087">將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs]\*\* 資料夾 (例如 `.\logs\stdout`)。</span><span class="sxs-lookup"><span data-stu-id="18747-1087">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="18747-1088">路徑中的 `stdout` 是記錄檔名稱前置詞。</span><span class="sxs-lookup"><span data-stu-id="18747-1088">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="18747-1089">建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。</span><span class="sxs-lookup"><span data-stu-id="18747-1089">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="18747-1090">使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。</span><span class="sxs-lookup"><span data-stu-id="18747-1090">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="18747-1091">請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="18747-1091">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="18747-1092">儲存已更新的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-1092">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="18747-1093">對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="18747-1093">Make a request to the app.</span></span>
1. <span data-ttu-id="18747-1094">瀏覽至 [logs]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-1094">Navigate to the *logs* folder.</span></span> <span data-ttu-id="18747-1095">尋找並開啟最新的 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18747-1095">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="18747-1096">研究記錄檔以了解錯誤。</span><span class="sxs-lookup"><span data-stu-id="18747-1096">Study the log for errors.</span></span>

<span data-ttu-id="18747-1097">完成疑難排解時，請停用 stdout 記錄：</span><span class="sxs-lookup"><span data-stu-id="18747-1097">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="18747-1098">編輯 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-1098">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="18747-1099">將 **stdoutLogEnabled** 設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="18747-1099">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="18747-1100">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-1100">Save the file.</span></span>

<span data-ttu-id="18747-1101">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="18747-1101">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-1102">如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-1102">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="18747-1103">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="18747-1103">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="18747-1104">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="18747-1104">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="18747-1105">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="18747-1105">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="18747-1106">啟用開發人員例外頁面</span><span class="sxs-lookup"><span data-stu-id="18747-1106">Enable the Developer Exception Page</span></span>

<span data-ttu-id="18747-1107">在開發環境中，可以將 `ASPNETCORE_ENVIRONMENT` [環境變數新增至 web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) 來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-1107">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="18747-1108">只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="18747-1108">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="18747-1109">只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="18747-1109">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="18747-1110">進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="18747-1110">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="18747-1111">如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="18747-1111">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="18747-1112">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="18747-1112">Obtain data from an app</span></span>

<span data-ttu-id="18747-1113">若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。</span><span class="sxs-lookup"><span data-stu-id="18747-1113">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="18747-1114">如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。</span><span class="sxs-lookup"><span data-stu-id="18747-1114">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="18747-1115">慢速或暫停應用程式 (IIS)</span><span class="sxs-lookup"><span data-stu-id="18747-1115">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="18747-1116">*崩潰轉儲*是系統記憶體的快照,可幫助確定應用崩潰、啟動失敗或應用慢速的原因。</span><span class="sxs-lookup"><span data-stu-id="18747-1116">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="18747-1117">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="18747-1117">App crashes or encounters an exception</span></span>

<span data-ttu-id="18747-1118">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="18747-1118">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="18747-1119">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="18747-1119">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="18747-1120">應用程式集區必須具備該資料夾的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="18747-1120">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="18747-1121">執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="18747-1121">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="18747-1122">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-1122">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="18747-1123">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-1123">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="18747-1124">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-1124">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="18747-1125">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="18747-1125">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="18747-1126">如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-1126">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="18747-1127">如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：</span><span class="sxs-lookup"><span data-stu-id="18747-1127">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="18747-1128">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-1128">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="18747-1129">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="18747-1129">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="18747-1130">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="18747-1130">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="18747-1131">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="18747-1131">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="18747-1132">當應用*掛起*(停止回應但不崩潰)、在啟動期間失敗或正常運行時,請參閱[使用者模式轉儲檔:選擇最佳工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)以選擇適當的工具來生成轉儲。</span><span class="sxs-lookup"><span data-stu-id="18747-1132">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="18747-1133">分析傾印</span><span class="sxs-lookup"><span data-stu-id="18747-1133">Analyze the dump</span></span>

<span data-ttu-id="18747-1134">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="18747-1134">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="18747-1135">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="18747-1135">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="18747-1136">清除包快取</span><span class="sxs-lookup"><span data-stu-id="18747-1136">Clear package caches</span></span>

<span data-ttu-id="18747-1137">在升級開發電腦上的 .NET Core SDK 或更改應用中的包版本後,正常運行的應用可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="18747-1137">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="18747-1138">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="18747-1138">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="18747-1139">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="18747-1139">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="18747-1140">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18747-1140">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="18747-1141">通過執行[dotnet nuget 本地變數](/dotnet/core/tools/dotnet-nuget-locals)來清除包快取 -- 從命令 shell 中清除。</span><span class="sxs-lookup"><span data-stu-id="18747-1141">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="18747-1142">清除包快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具`nuget locals all -clear`執行命令 。</span><span class="sxs-lookup"><span data-stu-id="18747-1142">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="18747-1143">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="18747-1143">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="18747-1144">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="18747-1144">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="18747-1145">在重新部署應用之前,請刪除伺服器上的部署資料夾中的所有檔。</span><span class="sxs-lookup"><span data-stu-id="18747-1145">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="18747-1146">其他資源</span><span class="sxs-lookup"><span data-stu-id="18747-1146">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="18747-1147">Azure 文件</span><span class="sxs-lookup"><span data-stu-id="18747-1147">Azure documentation</span></span>

* [<span data-ttu-id="18747-1148">ASP.NET Core 的 Application Insights</span><span class="sxs-lookup"><span data-stu-id="18747-1148">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="18747-1149">使用視覺化工作室在 Azure 應用服務中排除 Web 應用故障的遠端除錯 Web 應用部分</span><span class="sxs-lookup"><span data-stu-id="18747-1149">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="18747-1150">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="18747-1150">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="18747-1151">作法：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-1151">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="18747-1152">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="18747-1152">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="18747-1153">對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-1153">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="18747-1154">針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="18747-1154">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="18747-1155">Azure Web 應用程式的應用程式效能常見問題集</span><span class="sxs-lookup"><span data-stu-id="18747-1155">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* <span data-ttu-id="18747-1156">[Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="18747-1156">[Azure Web App sandbox (App Service runtime execution limitations)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)</span></span>
* [<span data-ttu-id="18747-1157">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="18747-1157">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="18747-1158">Visual Studio 文件</span><span class="sxs-lookup"><span data-stu-id="18747-1158">Visual Studio documentation</span></span>

* [<span data-ttu-id="18747-1159">遠端除錯ASP.NET視覺化工作室 2017 中 Azure 中的 IIS 核心</span><span class="sxs-lookup"><span data-stu-id="18747-1159">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="18747-1160">遠端除錯ASP.NET視覺化工作室遠端 IIS 電腦上的核心</span><span class="sxs-lookup"><span data-stu-id="18747-1160">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="18747-1161">使用視覺化工作室學習除錯</span><span class="sxs-lookup"><span data-stu-id="18747-1161">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="18747-1162">視覺化工作室代碼文件</span><span class="sxs-lookup"><span data-stu-id="18747-1162">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="18747-1163">使用 Visual Studio Code 偵錯</span><span class="sxs-lookup"><span data-stu-id="18747-1163">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)

::: moniker-end
