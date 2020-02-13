---
title: Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考
author: guardrex
description: 取得在 Azure Apps Service 與 IIS 上裝載 ASP.NET Core 應用程式時的常見錯誤疑難排解建議。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: dcc0f15c3f4a2747da744e98fe8fbcd3f325b709
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172433"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="16886-103">Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考</span><span class="sxs-lookup"><span data-stu-id="16886-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="16886-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="16886-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="16886-105">本主題描述常見的錯誤，並提供在 Azure App Service 和 IIS 上裝載 ASP.NET Core 應用程式時，針對特定錯誤的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="16886-105">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="16886-106">如需一般疑難排解指導方針，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-106">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="16886-107">收集下列資訊：</span><span class="sxs-lookup"><span data-stu-id="16886-107">Collect the following information:</span></span>

* <span data-ttu-id="16886-108">瀏覽器行為 (狀態碼和錯誤訊息)</span><span class="sxs-lookup"><span data-stu-id="16886-108">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="16886-109">應用程式事件記錄檔項目</span><span class="sxs-lookup"><span data-stu-id="16886-109">Application Event Log entries</span></span>
  * <span data-ttu-id="16886-110">Azure App Service &ndash; 請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-110">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="16886-111">IIS</span><span class="sxs-lookup"><span data-stu-id="16886-111">IIS</span></span>
    1. <span data-ttu-id="16886-112">在 [Windows] 功能表中選取 [開始]，鍵入「事件檢視器」，然後按 **Enter** 鍵。</span><span class="sxs-lookup"><span data-stu-id="16886-112">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="16886-113">在**事件檢視器**開啟之後，在提要欄位中展開 [ **Windows 記錄**] [>**應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="16886-113">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="16886-114">ASP.NET Core 模組 stdout 和偵錯記錄項目</span><span class="sxs-lookup"><span data-stu-id="16886-114">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="16886-115">Azure App Service &ndash; 請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-115">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="16886-116">IIS &ndash; 遵循＜ASP.NET Core 模組＞主題中[記錄檔建立和重新導向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)和[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)小節的指示。</span><span class="sxs-lookup"><span data-stu-id="16886-116">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="16886-117">將錯誤資訊與下列常見錯誤進行比較。</span><span class="sxs-lookup"><span data-stu-id="16886-117">Compare error information to the following common errors.</span></span> <span data-ttu-id="16886-118">如果找到相符情況，請依照疑難排解建議進行操作。</span><span class="sxs-lookup"><span data-stu-id="16886-118">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="16886-119">本主題中的錯誤清單並不完整。</span><span class="sxs-lookup"><span data-stu-id="16886-119">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="16886-120">如果您遇到這裡未列出的錯誤，請使用本主題底部的 [內容意見反應] 按鈕來開啟新的問題，並提供如何重現錯誤的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="16886-120">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="16886-121">作業系統升級已移除 32 位元的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="16886-121">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="16886-122">**應用程式記錄檔：** 無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。</span><span class="sxs-lookup"><span data-stu-id="16886-122">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="16886-123">資料錯誤。</span><span class="sxs-lookup"><span data-stu-id="16886-123">The data is the error.</span></span>

<span data-ttu-id="16886-124">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-124">Troubleshooting:</span></span>

<span data-ttu-id="16886-125">作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。</span><span class="sxs-lookup"><span data-stu-id="16886-125">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="16886-126">如果先安裝 ASP.NET Core 模組再升級 OS，然後在 OS 升級之後以 32 位元模式執行任何應用程式集區，就會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="16886-126">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="16886-127">作業系統升級之後，請修復 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="16886-127">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="16886-128">請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="16886-128">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="16886-129">執行安裝程式時，請選取 [修復]。</span><span class="sxs-lookup"><span data-stu-id="16886-129">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="16886-130">遺失網站延伸模組、已安裝 32 位元 (x86) 和 64 位元 (x64) 網站延伸模組，或錯誤的處理序位元集合</span><span class="sxs-lookup"><span data-stu-id="16886-130">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="16886-131">*適用於 Azure 應用程式服務所裝載的應用程式。*</span><span class="sxs-lookup"><span data-stu-id="16886-131">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="16886-132">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="16886-132">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="16886-133">**應用程式記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="16886-133">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="16886-134">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="16886-134">Could not find inprocess request handler.</span></span> <span data-ttu-id="16886-135">從叫用用 hostfxr 捕捉的輸出：找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="16886-135">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="16886-136">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="16886-136">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="16886-137">無法啟動應用程式 '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="16886-137">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="16886-138">**ASP.NET Core 模組 Stdout 記錄檔：** 找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="16886-138">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="16886-139">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="16886-139">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

* <span data-ttu-id="16886-140">**ASP.NET Core 模組的 Debug 記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="16886-140">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="16886-141">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="16886-141">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="16886-142">失敗的 HRESULT 傳回：0x8000ffff。</span><span class="sxs-lookup"><span data-stu-id="16886-142">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="16886-143">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="16886-143">Could not find inprocess request handler.</span></span> <span data-ttu-id="16886-144">無法找到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="16886-144">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="16886-145">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="16886-145">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

<span data-ttu-id="16886-146">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-146">Troubleshooting:</span></span>

* <span data-ttu-id="16886-147">如果在預覽執行階段中執行應用程式，請安裝與應用程式位元和應用程式執行階段版本相符的 32 位元 (x86) **或** 64 位元 (x64) 網站延伸模組。</span><span class="sxs-lookup"><span data-stu-id="16886-147">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="16886-148">**請勿同時安裝這兩個延伸模組或延伸模組的多個執行階段版本。**</span><span class="sxs-lookup"><span data-stu-id="16886-148">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="16886-149">ASP.NET Core {RUNTIME VERSION} (x86) 執行階段</span><span class="sxs-lookup"><span data-stu-id="16886-149">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="16886-150">ASP.NET Core {RUNTIME VERSION} (x64) 執行階段</span><span class="sxs-lookup"><span data-stu-id="16886-150">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="16886-151">重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="16886-151">Restart the app.</span></span> <span data-ttu-id="16886-152">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="16886-152">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="16886-153">如果在預覽執行階段中執行應用程式，而且已經安裝 32 位元 (x86) 和 64 位元 (x64) [網站延伸模組](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension)，請將與應用程式位元不符的網站延伸模組解除安裝。</span><span class="sxs-lookup"><span data-stu-id="16886-153">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="16886-154">移除網站延伸模組之後，請重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="16886-154">After removing the site extension, restart the app.</span></span> <span data-ttu-id="16886-155">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="16886-155">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="16886-156">如果在預覽執行階段中執行應用程式，且網站延伸模組位元與應用程式相符，請確認預覽網站延伸模組的「執行階段版本」是否與應用程式相符。</span><span class="sxs-lookup"><span data-stu-id="16886-156">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="16886-157">確認**應用程式設定**中應用程式的**平台**與應用程式位元相符。</span><span class="sxs-lookup"><span data-stu-id="16886-157">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="16886-158">如需詳細資訊，請參閱 <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>。</span><span class="sxs-lookup"><span data-stu-id="16886-158">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="16886-159">已部署 x86 應用程式，但未啟用 32 位元應用程式的應用程式集區</span><span class="sxs-lookup"><span data-stu-id="16886-159">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="16886-160">**瀏覽器：** HTTP 錯誤 500.30-ANCM 同進程啟動失敗</span><span class="sxs-lookup"><span data-stu-id="16886-160">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="16886-161">**應用程式記錄檔：** 實體根 ' {PATH} ' 的應用程式 '/LM/W3SVC/5/ROOT ' 遇到非預期的 managed 例外狀況，例外狀況代碼 = ' 0xe0434352 '。</span><span class="sxs-lookup"><span data-stu-id="16886-161">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="16886-162">如需詳細資訊，請檢查 stderr 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-162">Please check the stderr logs for more information.</span></span> <span data-ttu-id="16886-163">實體根目錄為 '{PATH}' 的應用程式 '/LM/W3SVC/5/ROOT' 無法載入 CLR 和受控應用程式。</span><span class="sxs-lookup"><span data-stu-id="16886-163">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="16886-164">CLR 背景工作執行緒不當結束</span><span class="sxs-lookup"><span data-stu-id="16886-164">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="16886-165">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="16886-165">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

* <span data-ttu-id="16886-166">**ASP.NET Core 模組的 Debug 記錄檔：** 失敗的 HRESULT 傳回：0x8007023e</span><span class="sxs-lookup"><span data-stu-id="16886-166">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

<span data-ttu-id="16886-167">SDK 會在發行獨立應用程式時攔截到此案例。</span><span class="sxs-lookup"><span data-stu-id="16886-167">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="16886-168">如果 RID 不符合平台目標 (例如 `win10-x64` RID 在專案檔中使用 `<PlatformTarget>x86</PlatformTarget>`)，SDK 就會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="16886-168">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="16886-169">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-169">Troubleshooting:</span></span>

<span data-ttu-id="16886-170">針對 x86 架構相依部署 (`<PlatformTarget>x86</PlatformTarget>`)，請啟用 32 位元應用程式的 IIS 應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="16886-170">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="16886-171">在 [IIS 管理員] 中，開啟應用程式集區的 [進階設定]，然後將 [啟用 32 位元應用程式] 設定為 [True]。</span><span class="sxs-lookup"><span data-stu-id="16886-171">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="16886-172">平台發生 RID 衝突</span><span class="sxs-lookup"><span data-stu-id="16886-172">Platform conflicts with RID</span></span>

* <span data-ttu-id="16886-173">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="16886-173">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="16886-174">**應用程式記錄檔：** 實體根目錄為 ' C：\{路徑} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} '\' 無法以命令列 ' "C：\{路徑} {ASSEMBLY} 啟動進程。{exe | dll} "'，ErrorCode = ' 0x80004005： ff。</span><span class="sxs-lookup"><span data-stu-id="16886-174">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="16886-175">**ASP.NET Core 模組 Stdout 記錄檔：** 未處理的例外狀況： System.badimageformatexception>：無法載入檔案或元件 ' {ASSEMBLY} .dll '。</span><span class="sxs-lookup"><span data-stu-id="16886-175">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="16886-176">嘗試載入了格式不正確的程式。</span><span class="sxs-lookup"><span data-stu-id="16886-176">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="16886-177">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-177">Troubleshooting:</span></span>

* <span data-ttu-id="16886-178">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="16886-178">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="16886-179">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="16886-179">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="16886-180">如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-180">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="16886-181">如果在升級應用程式和部署更新的組件時，Azure 應用程式部署發生這個例外狀況，請手動刪除來自先前部署的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="16886-181">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="16886-182">部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="16886-182">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="16886-183">URI 端點錯誤或停止了網站</span><span class="sxs-lookup"><span data-stu-id="16886-183">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="16886-184">**瀏覽器：** ERR_CONNECTION_REFUSED **--或--** 無法連接</span><span class="sxs-lookup"><span data-stu-id="16886-184">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="16886-185">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="16886-185">**Application Log:** No entry</span></span>

* <span data-ttu-id="16886-186">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-186">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="16886-187">**ASP.NET Core 模組的 Debug 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-187">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="16886-188">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-188">Troubleshooting:</span></span>

* <span data-ttu-id="16886-189">確認使用對應用程式正確的 URI 端點。</span><span class="sxs-lookup"><span data-stu-id="16886-189">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="16886-190">檢查繫結。</span><span class="sxs-lookup"><span data-stu-id="16886-190">Check the bindings.</span></span>

* <span data-ttu-id="16886-191">確認 IIS 網站不是處於「已停止」狀態。</span><span class="sxs-lookup"><span data-stu-id="16886-191">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="16886-192">已停用 CoreWebEngine 或 W3SVC 伺服器功能</span><span class="sxs-lookup"><span data-stu-id="16886-192">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="16886-193">**作業系統例外狀況：** 必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="16886-193">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="16886-194">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-194">Troubleshooting:</span></span>

<span data-ttu-id="16886-195">確認已啟用適當的角色和功能。</span><span class="sxs-lookup"><span data-stu-id="16886-195">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="16886-196">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="16886-196">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="16886-197">不正確的網站實體路徑或遺失應用程式</span><span class="sxs-lookup"><span data-stu-id="16886-197">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="16886-198">**瀏覽器：** 403 禁止 - 存取被拒 **--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="16886-198">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="16886-199">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="16886-199">**Application Log:** No entry</span></span>

* <span data-ttu-id="16886-200">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-200">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="16886-201">**ASP.NET Core 模組的 Debug 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-201">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="16886-202">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-202">Troubleshooting:</span></span>

<span data-ttu-id="16886-203">檢查 IIS 網站的 [基本設定] 和實體的應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="16886-203">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="16886-204">確認應用程式位於 IIS 網站**實體路徑**的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="16886-204">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="16886-205">角色不正確、ASP.NET Core 模組未安裝，或權限不正確</span><span class="sxs-lookup"><span data-stu-id="16886-205">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="16886-206">**瀏覽器：** 500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="16886-206">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="16886-207">**--或--** 無法顯示此頁面</span><span class="sxs-lookup"><span data-stu-id="16886-207">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="16886-208">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="16886-208">**Application Log:** No entry</span></span>

* <span data-ttu-id="16886-209">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-209">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="16886-210">**ASP.NET Core 模組的 Debug 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-210">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="16886-211">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-211">Troubleshooting:</span></span>

* <span data-ttu-id="16886-212">確認已啟用適當的角色。</span><span class="sxs-lookup"><span data-stu-id="16886-212">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="16886-213">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="16886-213">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="16886-214">開啟 [程式和功能] 或 [應用程式與功能]，並確認已安裝 **Windows Server Hosting**。</span><span class="sxs-lookup"><span data-stu-id="16886-214">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="16886-215">如果已安裝的程式清單中沒有 **Windows Server Hosting**，請下載並安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="16886-215">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="16886-216">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="16886-216">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="16886-217">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="16886-217">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="16886-218">請確定**應用程式**集區 >**進程模型**> 身分**識別**設定為**ApplicationPoolIdentity** ，或自訂身分識別具有正確的許可權可存取應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="16886-218">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="16886-219">若解除安裝 ASP.NET Core Hosting Bundle 並安裝舊版裝載套件組合，*applicationHost.config* 檔案不會包括 ASP.NET Core Module 的區段。</span><span class="sxs-lookup"><span data-stu-id="16886-219">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="16886-220">開啟位於 *%windir%/System32/inetsrv/config* 的 *applicationHost.config*，然後尋找 `<configuration><configSections><sectionGroup name="system.webServer">` 區段群組。</span><span class="sxs-lookup"><span data-stu-id="16886-220">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="16886-221">若區段群組中沒有 ASP.NET Core Module 的區段，請新增區段元素：</span><span class="sxs-lookup"><span data-stu-id="16886-221">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="16886-222">或者，您也可以安裝最新版的「ASP.NET Core 裝載套件組合」。</span><span class="sxs-lookup"><span data-stu-id="16886-222">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="16886-223">最新版本回溯相容，提供支援的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="16886-223">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="16886-224">不正確的 processPath、遺失 PATH 變數、未安裝裝載套件組合、未重新啟動系統/IIS、未安裝 VC++ 可轉散發套件或 dotnet.exe 存取違規</span><span class="sxs-lookup"><span data-stu-id="16886-224">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="16886-225">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="16886-225">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="16886-226">**應用程式記錄檔：** 實體根目錄為 ' C：\{PATH} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} '\' 無法以命令列 ' "{...}" 啟動進程</span><span class="sxs-lookup"><span data-stu-id="16886-226">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="16886-227">'，ErrorCode = ' 0x80070002：0。</span><span class="sxs-lookup"><span data-stu-id="16886-227">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="16886-228">應用程式 '{PATH}' 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="16886-228">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="16886-229">在 '{PATH}' 找不到可執行檔。</span><span class="sxs-lookup"><span data-stu-id="16886-229">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="16886-230">無法啟動應用程式 '/LM/W3SVC/2/ROOT'，ErrorCode '0x8007023e'。</span><span class="sxs-lookup"><span data-stu-id="16886-230">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="16886-231">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-231">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="16886-232">**ASP.NET Core 模組的 Debug 記錄檔：** 事件記錄檔： ' 應用程式 ' {PATH} ' 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="16886-232">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="16886-233">在 '{PATH}' 找不到可執行檔。</span><span class="sxs-lookup"><span data-stu-id="16886-233">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="16886-234">失敗的 HRESULT 傳回：0x8007023e</span><span class="sxs-lookup"><span data-stu-id="16886-234">Failed HRESULT returned: 0x8007023e</span></span>

<span data-ttu-id="16886-235">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-235">Troubleshooting:</span></span>

* <span data-ttu-id="16886-236">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="16886-236">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="16886-237">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="16886-237">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="16886-238">如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-238">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="16886-239">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *processPath* 屬性，以確認它是 `dotnet` (適用於架構相依部署 (FDD)) 或 `.\{ASSEMBLY}.exe` (適用於[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd))。</span><span class="sxs-lookup"><span data-stu-id="16886-239">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="16886-240">若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="16886-240">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="16886-241">確認系統 PATH 設定中有 *C:\Program Files\dotnet\\* 。</span><span class="sxs-lookup"><span data-stu-id="16886-241">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="16886-242">若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="16886-242">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="16886-243">確認應用程式集區使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。</span><span class="sxs-lookup"><span data-stu-id="16886-243">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="16886-244">確認 *C:\Program Files\dotnet* 和應用程式目錄上未針對應用程式集區使用者身分識別設定任何拒絕規則。</span><span class="sxs-lookup"><span data-stu-id="16886-244">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="16886-245">可能已部署 FDD 並安裝 .NET Core，但未重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="16886-245">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="16886-246">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="16886-246">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="16886-247">可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="16886-247">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="16886-248">如果尚未安裝 .NET Core 執行階段，請在系統上執行「.NET Core 裝載套件組合安裝程式」。</span><span class="sxs-lookup"><span data-stu-id="16886-248">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="16886-249">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="16886-249">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="16886-250">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="16886-250">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="16886-251">如果需要特定執行階段，請從 [.NET Download Archives](https://dotnet.microsoft.com/download/archives) (.NET 下載封存) 下載執行階段，並將它安裝在系統上。</span><span class="sxs-lookup"><span data-stu-id="16886-251">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="16886-252">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。</span><span class="sxs-lookup"><span data-stu-id="16886-252">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="16886-253">不正確的 \<aspNetCore> 元素引數</span><span class="sxs-lookup"><span data-stu-id="16886-253">Incorrect arguments of \<aspNetCore> element</span></span>

* <span data-ttu-id="16886-254">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="16886-254">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="16886-255">**應用程式記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="16886-255">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="16886-256">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="16886-256">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="16886-257">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="16886-257">Could not find inprocess request handler.</span></span> <span data-ttu-id="16886-258">從叫用用 hostfxr 捕獲的輸出：您是否說要執行 dotnet SDK 命令？</span><span class="sxs-lookup"><span data-stu-id="16886-258">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="16886-259">請從下列安裝 dotnet SDK： https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 無法啟動應用程式 '/LM/W3SVC/3/ROOT '，ErrorCode ' 0x8000ffff '。</span><span class="sxs-lookup"><span data-stu-id="16886-259">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="16886-260">**ASP.NET Core 模組 Stdout 記錄檔：** 您是否表示要執行 dotnet SDK 命令？</span><span class="sxs-lookup"><span data-stu-id="16886-260">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="16886-261">請從下列位置安裝 dotnet SDK： https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="16886-261">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="16886-262">**ASP.NET Core 模組的 Debug 記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="16886-262">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="16886-263">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="16886-263">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="16886-264">失敗的 HRESULT 傳回：0x8000ffff 找不到進程間要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="16886-264">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="16886-265">從叫用用 hostfxr 捕獲的輸出：您是否說要執行 dotnet SDK 命令？</span><span class="sxs-lookup"><span data-stu-id="16886-265">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="16886-266">請從下列安裝 dotnet SDK： https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT 傳回：0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="16886-266">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

<span data-ttu-id="16886-267">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-267">Troubleshooting:</span></span>

* <span data-ttu-id="16886-268">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="16886-268">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="16886-269">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="16886-269">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="16886-270">如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-270">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="16886-271">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *arguments* 屬性，以確認它是 (a) `.\{ASSEMBLY}.dll` (適用於架構相依部署 (FDD))；或 (b) 不存在、空字串 (`arguments=""`)，或是應用程式的引數清單 (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`，適用於自封式部署 (SCD))。</span><span class="sxs-lookup"><span data-stu-id="16886-271">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="16886-272">遺失 .NET Core 共用架構</span><span class="sxs-lookup"><span data-stu-id="16886-272">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="16886-273">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="16886-273">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="16886-274">**應用程式記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="16886-274">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="16886-275">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="16886-275">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="16886-276">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="16886-276">Could not find inprocess request handler.</span></span> <span data-ttu-id="16886-277">從叫用用 hostfxr 捕捉的輸出：找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="16886-277">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="16886-278">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}'。</span><span class="sxs-lookup"><span data-stu-id="16886-278">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="16886-279">無法啟動應用程式 '/LM/W3SVC/5/ROOT'，ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="16886-279">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="16886-280">**ASP.NET Core 模組 Stdout 記錄檔：** 找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="16886-280">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="16886-281">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}'。</span><span class="sxs-lookup"><span data-stu-id="16886-281">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="16886-282">**ASP.NET Core 模組的 Debug 記錄檔：** 失敗的 HRESULT 傳回：0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="16886-282">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

<span data-ttu-id="16886-283">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-283">Troubleshooting:</span></span>

<span data-ttu-id="16886-284">若為結構相依部署 (FDD)，請確認已在系統上安裝正確的執行階段。</span><span class="sxs-lookup"><span data-stu-id="16886-284">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="16886-285">已停止應用程式集區</span><span class="sxs-lookup"><span data-stu-id="16886-285">Stopped Application Pool</span></span>

* <span data-ttu-id="16886-286">**瀏覽器：** 503 服務無法使用</span><span class="sxs-lookup"><span data-stu-id="16886-286">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="16886-287">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="16886-287">**Application Log:** No entry</span></span>

* <span data-ttu-id="16886-288">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-288">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="16886-289">**ASP.NET Core 模組的 Debug 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-289">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="16886-290">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-290">Troubleshooting:</span></span>

<span data-ttu-id="16886-291">確認「應用程式集區」不是處於「已停止」狀態。</span><span class="sxs-lookup"><span data-stu-id="16886-291">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="16886-292">子應用程式包含 \<handlers> 區段</span><span class="sxs-lookup"><span data-stu-id="16886-292">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="16886-293">**瀏覽器：** HTTP 錯誤 500.19 - 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="16886-293">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="16886-294">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="16886-294">**Application Log:** No entry</span></span>

* <span data-ttu-id="16886-295">**ASP.NET Core 模組 Stdout 記錄檔：** 根應用程式的記錄檔隨即建立，並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="16886-295">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="16886-296">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-296">The sub-app's log file isn't created.</span></span>

* <span data-ttu-id="16886-297">**ASP.NET Core 模組的 Debug 記錄檔：** 根應用程式的記錄檔隨即建立，並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="16886-297">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="16886-298">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-298">The sub-app's log file isn't created.</span></span>

<span data-ttu-id="16886-299">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-299">Troubleshooting:</span></span>

<span data-ttu-id="16886-300">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段，或子應用程式未繼承父應用程式的處理常式。</span><span class="sxs-lookup"><span data-stu-id="16886-300">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="16886-301">父應用程式 `<system.webServer>`web.config*的* 區段位於 `<location>` 元素內。</span><span class="sxs-lookup"><span data-stu-id="16886-301">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="16886-302">將 <xref:System.Configuration.SectionInformation.InheritInChildApplications*> 屬性設定為 `false`，以表示在 [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 元素內指定的設定，不是由位在父應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="16886-302">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="16886-303">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="16886-303">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="16886-304">stdout 記錄檔路徑不正確</span><span class="sxs-lookup"><span data-stu-id="16886-304">stdout log path incorrect</span></span>

* <span data-ttu-id="16886-305">**瀏覽器：** 應用程式正常回應。</span><span class="sxs-lookup"><span data-stu-id="16886-305">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="16886-306">**應用程式記錄檔：** 無法在 C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll 中啟動 stdout 重新導向</span><span class="sxs-lookup"><span data-stu-id="16886-306">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="16886-307">例外狀況訊息：已在 {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp：84傳回 HRESULT 0x80070005。</span><span class="sxs-lookup"><span data-stu-id="16886-307">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="16886-308">無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中停止 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="16886-308">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="16886-309">例外狀況訊息：在 {PATH} 傳回 HRESULT 0x80070002。</span><span class="sxs-lookup"><span data-stu-id="16886-309">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="16886-310">無法在 {PATH}\aspnetcorev2_inprocess.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="16886-310">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="16886-311">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-311">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="16886-312">**ASP.NET Core 模組的 Debug 記錄檔：** 無法在 C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll 中啟動 stdout 重新導向</span><span class="sxs-lookup"><span data-stu-id="16886-312">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="16886-313">例外狀況訊息：已在 {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp：84傳回 HRESULT 0x80070005。</span><span class="sxs-lookup"><span data-stu-id="16886-313">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="16886-314">無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中停止 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="16886-314">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="16886-315">例外狀況訊息：在 {PATH} 傳回 HRESULT 0x80070002。</span><span class="sxs-lookup"><span data-stu-id="16886-315">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="16886-316">無法在 {PATH}\aspnetcorev2_inprocess.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="16886-316">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

<span data-ttu-id="16886-317">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-317">Troubleshooting:</span></span>

* <span data-ttu-id="16886-318">`stdoutLogFile`web.config`<aspNetCore>` 中 *元素所指定的* 路徑不存在。</span><span class="sxs-lookup"><span data-stu-id="16886-318">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="16886-319">如需詳細資訊，請參閱[ASP.NET Core 模組：記錄建立和](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)重新導向。</span><span class="sxs-lookup"><span data-stu-id="16886-319">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="16886-320">應用程式集區使用者沒有 stdout 記錄檔路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="16886-320">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="16886-321">應用程式組態一般問題</span><span class="sxs-lookup"><span data-stu-id="16886-321">Application configuration general issue</span></span>

* <span data-ttu-id="16886-322">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗 **--或--** HTTP 錯誤 500.30-ANCM 同進程啟動失敗</span><span class="sxs-lookup"><span data-stu-id="16886-322">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="16886-323">**應用程式記錄檔：** 變</span><span class="sxs-lookup"><span data-stu-id="16886-323">**Application Log:** Variable</span></span>

* <span data-ttu-id="16886-324">**ASP.NET Core 模組 Stdout 記錄檔：** 在應用程式失敗之前，會建立記錄檔，但卻是空的或以一般專案建立的。</span><span class="sxs-lookup"><span data-stu-id="16886-324">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="16886-325">**ASP.NET Core 模組的 Debug 記錄檔：** 變</span><span class="sxs-lookup"><span data-stu-id="16886-325">**ASP.NET Core Module Debug Log:** Variable</span></span>

<span data-ttu-id="16886-326">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-326">Troubleshooting:</span></span>

<span data-ttu-id="16886-327">處理序無法啟動，最可能的原因是應用程式組態或程式設計問題。</span><span class="sxs-lookup"><span data-stu-id="16886-327">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="16886-328">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="16886-328">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="16886-329">本主題描述常見的錯誤，並提供在 Azure App Service 和 IIS 上裝載 ASP.NET Core 應用程式時，針對特定錯誤的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="16886-329">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="16886-330">如需一般疑難排解指導方針，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-330">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="16886-331">收集下列資訊：</span><span class="sxs-lookup"><span data-stu-id="16886-331">Collect the following information:</span></span>

* <span data-ttu-id="16886-332">瀏覽器行為 (狀態碼和錯誤訊息)</span><span class="sxs-lookup"><span data-stu-id="16886-332">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="16886-333">應用程式事件記錄檔項目</span><span class="sxs-lookup"><span data-stu-id="16886-333">Application Event Log entries</span></span>
  * <span data-ttu-id="16886-334">Azure App Service &ndash; 請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-334">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="16886-335">IIS</span><span class="sxs-lookup"><span data-stu-id="16886-335">IIS</span></span>
    1. <span data-ttu-id="16886-336">在 [Windows] 功能表中選取 [開始]，鍵入「事件檢視器」，然後按 **Enter** 鍵。</span><span class="sxs-lookup"><span data-stu-id="16886-336">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="16886-337">在**事件檢視器**開啟之後，在提要欄位中展開 [ **Windows 記錄**] [>**應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="16886-337">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="16886-338">ASP.NET Core 模組 stdout 和偵錯記錄項目</span><span class="sxs-lookup"><span data-stu-id="16886-338">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="16886-339">Azure App Service &ndash; 請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-339">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="16886-340">IIS &ndash; 遵循＜ASP.NET Core 模組＞主題中[記錄檔建立和重新導向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)和[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)小節的指示。</span><span class="sxs-lookup"><span data-stu-id="16886-340">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="16886-341">將錯誤資訊與下列常見錯誤進行比較。</span><span class="sxs-lookup"><span data-stu-id="16886-341">Compare error information to the following common errors.</span></span> <span data-ttu-id="16886-342">如果找到相符情況，請依照疑難排解建議進行操作。</span><span class="sxs-lookup"><span data-stu-id="16886-342">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="16886-343">本主題中的錯誤清單並不完整。</span><span class="sxs-lookup"><span data-stu-id="16886-343">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="16886-344">如果您遇到這裡未列出的錯誤，請使用本主題底部的 [內容意見反應] 按鈕來開啟新的問題，並提供如何重現錯誤的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="16886-344">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="16886-345">作業系統升級已移除 32 位元的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="16886-345">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="16886-346">**應用程式記錄檔：** 無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。</span><span class="sxs-lookup"><span data-stu-id="16886-346">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="16886-347">資料錯誤。</span><span class="sxs-lookup"><span data-stu-id="16886-347">The data is the error.</span></span>

<span data-ttu-id="16886-348">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-348">Troubleshooting:</span></span>

<span data-ttu-id="16886-349">作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。</span><span class="sxs-lookup"><span data-stu-id="16886-349">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="16886-350">如果先安裝 ASP.NET Core 模組再升級 OS，然後在 OS 升級之後以 32 位元模式執行任何應用程式集區，就會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="16886-350">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="16886-351">作業系統升級之後，請修復 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="16886-351">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="16886-352">請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="16886-352">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="16886-353">執行安裝程式時，請選取 [修復]。</span><span class="sxs-lookup"><span data-stu-id="16886-353">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="16886-354">遺失網站延伸模組、已安裝 32 位元 (x86) 和 64 位元 (x64) 網站延伸模組，或錯誤的處理序位元集合</span><span class="sxs-lookup"><span data-stu-id="16886-354">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="16886-355">*適用於 Azure 應用程式服務所裝載的應用程式。*</span><span class="sxs-lookup"><span data-stu-id="16886-355">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="16886-356">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="16886-356">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="16886-357">**應用程式記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="16886-357">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="16886-358">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="16886-358">Could not find inprocess request handler.</span></span> <span data-ttu-id="16886-359">從叫用用 hostfxr 捕捉的輸出：找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="16886-359">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="16886-360">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="16886-360">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="16886-361">無法啟動應用程式 '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="16886-361">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="16886-362">**ASP.NET Core 模組 Stdout 記錄檔：** 找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="16886-362">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="16886-363">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="16886-363">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

<span data-ttu-id="16886-364">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-364">Troubleshooting:</span></span>

* <span data-ttu-id="16886-365">如果在預覽執行階段中執行應用程式，請安裝與應用程式位元和應用程式執行階段版本相符的 32 位元 (x86) **或** 64 位元 (x64) 網站延伸模組。</span><span class="sxs-lookup"><span data-stu-id="16886-365">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="16886-366">**請勿同時安裝這兩個延伸模組或延伸模組的多個執行階段版本。**</span><span class="sxs-lookup"><span data-stu-id="16886-366">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="16886-367">ASP.NET Core {RUNTIME VERSION} (x86) 執行階段</span><span class="sxs-lookup"><span data-stu-id="16886-367">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="16886-368">ASP.NET Core {RUNTIME VERSION} (x64) 執行階段</span><span class="sxs-lookup"><span data-stu-id="16886-368">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="16886-369">重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="16886-369">Restart the app.</span></span> <span data-ttu-id="16886-370">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="16886-370">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="16886-371">如果在預覽執行階段中執行應用程式，而且已經安裝 32 位元 (x86) 和 64 位元 (x64) [網站延伸模組](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension)，請將與應用程式位元不符的網站延伸模組解除安裝。</span><span class="sxs-lookup"><span data-stu-id="16886-371">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="16886-372">移除網站延伸模組之後，請重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="16886-372">After removing the site extension, restart the app.</span></span> <span data-ttu-id="16886-373">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="16886-373">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="16886-374">如果在預覽執行階段中執行應用程式，且網站延伸模組位元與應用程式相符，請確認預覽網站延伸模組的「執行階段版本」是否與應用程式相符。</span><span class="sxs-lookup"><span data-stu-id="16886-374">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="16886-375">確認**應用程式設定**中應用程式的**平台**與應用程式位元相符。</span><span class="sxs-lookup"><span data-stu-id="16886-375">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="16886-376">如需詳細資訊，請參閱 <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>。</span><span class="sxs-lookup"><span data-stu-id="16886-376">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="16886-377">已部署 x86 應用程式，但未啟用 32 位元應用程式的應用程式集區</span><span class="sxs-lookup"><span data-stu-id="16886-377">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="16886-378">**瀏覽器：** HTTP 錯誤 500.30-ANCM 同進程啟動失敗</span><span class="sxs-lookup"><span data-stu-id="16886-378">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="16886-379">**應用程式記錄檔：** 實體根 ' {PATH} ' 的應用程式 '/LM/W3SVC/5/ROOT ' 遇到非預期的 managed 例外狀況，例外狀況代碼 = ' 0xe0434352 '。</span><span class="sxs-lookup"><span data-stu-id="16886-379">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="16886-380">如需詳細資訊，請檢查 stderr 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-380">Please check the stderr logs for more information.</span></span> <span data-ttu-id="16886-381">實體根目錄為 '{PATH}' 的應用程式 '/LM/W3SVC/5/ROOT' 無法載入 CLR 和受控應用程式。</span><span class="sxs-lookup"><span data-stu-id="16886-381">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="16886-382">CLR 背景工作執行緒不當結束</span><span class="sxs-lookup"><span data-stu-id="16886-382">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="16886-383">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="16886-383">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

<span data-ttu-id="16886-384">SDK 會在發行獨立應用程式時攔截到此案例。</span><span class="sxs-lookup"><span data-stu-id="16886-384">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="16886-385">如果 RID 不符合平台目標 (例如 `win10-x64` RID 在專案檔中使用 `<PlatformTarget>x86</PlatformTarget>`)，SDK 就會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="16886-385">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="16886-386">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-386">Troubleshooting:</span></span>

<span data-ttu-id="16886-387">針對 x86 架構相依部署 (`<PlatformTarget>x86</PlatformTarget>`)，請啟用 32 位元應用程式的 IIS 應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="16886-387">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="16886-388">在 [IIS 管理員] 中，開啟應用程式集區的 [進階設定]，然後將 [啟用 32 位元應用程式] 設定為 [True]。</span><span class="sxs-lookup"><span data-stu-id="16886-388">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="16886-389">平台發生 RID 衝突</span><span class="sxs-lookup"><span data-stu-id="16886-389">Platform conflicts with RID</span></span>

* <span data-ttu-id="16886-390">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="16886-390">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="16886-391">**應用程式記錄檔：** 實體根目錄為 ' C：\{路徑} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} '\' 無法以命令列 ' "C：\{路徑} {ASSEMBLY} 啟動進程。{exe | dll} "'，ErrorCode = ' 0x80004005： ff。</span><span class="sxs-lookup"><span data-stu-id="16886-391">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="16886-392">**ASP.NET Core 模組 Stdout 記錄檔：** 未處理的例外狀況： System.badimageformatexception>：無法載入檔案或元件 ' {ASSEMBLY} .dll '。</span><span class="sxs-lookup"><span data-stu-id="16886-392">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="16886-393">嘗試載入了格式不正確的程式。</span><span class="sxs-lookup"><span data-stu-id="16886-393">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="16886-394">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-394">Troubleshooting:</span></span>

* <span data-ttu-id="16886-395">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="16886-395">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="16886-396">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="16886-396">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="16886-397">如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-397">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="16886-398">如果在升級應用程式和部署更新的組件時，Azure 應用程式部署發生這個例外狀況，請手動刪除來自先前部署的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="16886-398">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="16886-399">部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="16886-399">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="16886-400">URI 端點錯誤或停止了網站</span><span class="sxs-lookup"><span data-stu-id="16886-400">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="16886-401">**瀏覽器：** ERR_CONNECTION_REFUSED **--或--** 無法連接</span><span class="sxs-lookup"><span data-stu-id="16886-401">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="16886-402">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="16886-402">**Application Log:** No entry</span></span>

* <span data-ttu-id="16886-403">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-403">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="16886-404">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-404">Troubleshooting:</span></span>

* <span data-ttu-id="16886-405">確認使用對應用程式正確的 URI 端點。</span><span class="sxs-lookup"><span data-stu-id="16886-405">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="16886-406">檢查繫結。</span><span class="sxs-lookup"><span data-stu-id="16886-406">Check the bindings.</span></span>

* <span data-ttu-id="16886-407">確認 IIS 網站不是處於「已停止」狀態。</span><span class="sxs-lookup"><span data-stu-id="16886-407">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="16886-408">已停用 CoreWebEngine 或 W3SVC 伺服器功能</span><span class="sxs-lookup"><span data-stu-id="16886-408">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="16886-409">**作業系統例外狀況：** 必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="16886-409">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="16886-410">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-410">Troubleshooting:</span></span>

<span data-ttu-id="16886-411">確認已啟用適當的角色和功能。</span><span class="sxs-lookup"><span data-stu-id="16886-411">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="16886-412">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="16886-412">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="16886-413">不正確的網站實體路徑或遺失應用程式</span><span class="sxs-lookup"><span data-stu-id="16886-413">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="16886-414">**瀏覽器：** 403 禁止 - 存取被拒 **--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="16886-414">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="16886-415">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="16886-415">**Application Log:** No entry</span></span>

* <span data-ttu-id="16886-416">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-416">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="16886-417">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-417">Troubleshooting:</span></span>

<span data-ttu-id="16886-418">檢查 IIS 網站的 [基本設定] 和實體的應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="16886-418">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="16886-419">確認應用程式位於 IIS 網站**實體路徑**的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="16886-419">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="16886-420">角色不正確、ASP.NET Core 模組未安裝，或權限不正確</span><span class="sxs-lookup"><span data-stu-id="16886-420">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="16886-421">**瀏覽器：** 500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="16886-421">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="16886-422">**--或--** 無法顯示此頁面</span><span class="sxs-lookup"><span data-stu-id="16886-422">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="16886-423">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="16886-423">**Application Log:** No entry</span></span>

* <span data-ttu-id="16886-424">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-424">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="16886-425">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-425">Troubleshooting:</span></span>

* <span data-ttu-id="16886-426">確認已啟用適當的角色。</span><span class="sxs-lookup"><span data-stu-id="16886-426">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="16886-427">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="16886-427">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="16886-428">開啟 [程式和功能] 或 [應用程式與功能]，並確認已安裝 **Windows Server Hosting**。</span><span class="sxs-lookup"><span data-stu-id="16886-428">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="16886-429">如果已安裝的程式清單中沒有 **Windows Server Hosting**，請下載並安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="16886-429">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="16886-430">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="16886-430">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="16886-431">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="16886-431">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="16886-432">請確定**應用程式**集區 >**進程模型**> 身分**識別**設定為**ApplicationPoolIdentity** ，或自訂身分識別具有正確的許可權可存取應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="16886-432">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="16886-433">若解除安裝 ASP.NET Core Hosting Bundle 並安裝舊版裝載套件組合，*applicationHost.config* 檔案不會包括 ASP.NET Core Module 的區段。</span><span class="sxs-lookup"><span data-stu-id="16886-433">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="16886-434">開啟位於 *%windir%/System32/inetsrv/config* 的 *applicationHost.config*，然後尋找 `<configuration><configSections><sectionGroup name="system.webServer">` 區段群組。</span><span class="sxs-lookup"><span data-stu-id="16886-434">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="16886-435">若區段群組中沒有 ASP.NET Core Module 的區段，請新增區段元素：</span><span class="sxs-lookup"><span data-stu-id="16886-435">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="16886-436">或者，您也可以安裝最新版的「ASP.NET Core 裝載套件組合」。</span><span class="sxs-lookup"><span data-stu-id="16886-436">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="16886-437">最新版本回溯相容，提供支援的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="16886-437">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="16886-438">不正確的 processPath、遺失 PATH 變數、未安裝裝載套件組合、未重新啟動系統/IIS、未安裝 VC++ 可轉散發套件或 dotnet.exe 存取違規</span><span class="sxs-lookup"><span data-stu-id="16886-438">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="16886-439">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="16886-439">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="16886-440">**應用程式記錄檔：** 實體根目錄為 ' C：\{PATH} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} '\' 無法以命令列 ' "{...}" 啟動進程</span><span class="sxs-lookup"><span data-stu-id="16886-440">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="16886-441">'，ErrorCode = ' 0x80070002：0。</span><span class="sxs-lookup"><span data-stu-id="16886-441">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="16886-442">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="16886-442">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

<span data-ttu-id="16886-443">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-443">Troubleshooting:</span></span>

* <span data-ttu-id="16886-444">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="16886-444">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="16886-445">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="16886-445">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="16886-446">如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-446">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="16886-447">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *processPath* 屬性，以確認它是 `dotnet` (適用於架構相依部署 (FDD)) 或 `.\{ASSEMBLY}.exe` (適用於[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd))。</span><span class="sxs-lookup"><span data-stu-id="16886-447">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="16886-448">若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="16886-448">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="16886-449">確認系統 PATH 設定中有 *C:\Program Files\dotnet\\* 。</span><span class="sxs-lookup"><span data-stu-id="16886-449">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="16886-450">若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="16886-450">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="16886-451">確認應用程式集區使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。</span><span class="sxs-lookup"><span data-stu-id="16886-451">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="16886-452">確認 *C:\Program Files\dotnet* 和應用程式目錄上未針對應用程式集區使用者身分識別設定任何拒絕規則。</span><span class="sxs-lookup"><span data-stu-id="16886-452">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="16886-453">可能已部署 FDD 並安裝 .NET Core，但未重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="16886-453">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="16886-454">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="16886-454">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="16886-455">可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="16886-455">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="16886-456">如果尚未安裝 .NET Core 執行階段，請在系統上執行「.NET Core 裝載套件組合安裝程式」。</span><span class="sxs-lookup"><span data-stu-id="16886-456">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="16886-457">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="16886-457">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="16886-458">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="16886-458">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="16886-459">如果需要特定執行階段，請從 [.NET Download Archives](https://dotnet.microsoft.com/download/archives) (.NET 下載封存) 下載執行階段，並將它安裝在系統上。</span><span class="sxs-lookup"><span data-stu-id="16886-459">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="16886-460">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。</span><span class="sxs-lookup"><span data-stu-id="16886-460">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="16886-461">不正確的 \<aspNetCore> 元素引數</span><span class="sxs-lookup"><span data-stu-id="16886-461">Incorrect arguments of \<aspNetCore> element</span></span>

* <span data-ttu-id="16886-462">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="16886-462">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="16886-463">**應用程式記錄檔：** 實體根目錄為 ' C：\{PATH} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} '\' 無法以命令列 ' "dotnet" 啟動進程。\{ASSEMBLY} .dll '，ErrorCode = ' 0x80004005：80008081。</span><span class="sxs-lookup"><span data-stu-id="16886-463">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="16886-464">**ASP.NET Core 模組 Stdout 記錄檔：** 要執行的應用程式不存在： ' PATH\{ASSEMBLY} .dll '</span><span class="sxs-lookup"><span data-stu-id="16886-464">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

<span data-ttu-id="16886-465">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-465">Troubleshooting:</span></span>

* <span data-ttu-id="16886-466">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="16886-466">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="16886-467">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="16886-467">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="16886-468">如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="16886-468">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="16886-469">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *arguments* 屬性，以確認它是 (a) `.\{ASSEMBLY}.dll` (適用於架構相依部署 (FDD))；或 (b) 不存在、空字串 (`arguments=""`)，或是應用程式的引數清單 (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`，適用於自封式部署 (SCD))。</span><span class="sxs-lookup"><span data-stu-id="16886-469">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

<span data-ttu-id="16886-470">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-470">Troubleshooting:</span></span>

<span data-ttu-id="16886-471">若為結構相依部署 (FDD)，請確認已在系統上安裝正確的執行階段。</span><span class="sxs-lookup"><span data-stu-id="16886-471">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="16886-472">已停止應用程式集區</span><span class="sxs-lookup"><span data-stu-id="16886-472">Stopped Application Pool</span></span>

* <span data-ttu-id="16886-473">**瀏覽器：** 503 服務無法使用</span><span class="sxs-lookup"><span data-stu-id="16886-473">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="16886-474">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="16886-474">**Application Log:** No entry</span></span>

* <span data-ttu-id="16886-475">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-475">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="16886-476">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-476">Troubleshooting:</span></span>

<span data-ttu-id="16886-477">確認「應用程式集區」不是處於「已停止」狀態。</span><span class="sxs-lookup"><span data-stu-id="16886-477">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="16886-478">子應用程式包含 \<handlers> 區段</span><span class="sxs-lookup"><span data-stu-id="16886-478">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="16886-479">**瀏覽器：** HTTP 錯誤 500.19 - 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="16886-479">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="16886-480">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="16886-480">**Application Log:** No entry</span></span>

* <span data-ttu-id="16886-481">**ASP.NET Core 模組 Stdout 記錄檔：** 根應用程式的記錄檔隨即建立，並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="16886-481">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="16886-482">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-482">The sub-app's log file isn't created.</span></span>

<span data-ttu-id="16886-483">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-483">Troubleshooting:</span></span>

<span data-ttu-id="16886-484">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段。</span><span class="sxs-lookup"><span data-stu-id="16886-484">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="16886-485">stdout 記錄檔路徑不正確</span><span class="sxs-lookup"><span data-stu-id="16886-485">stdout log path incorrect</span></span>

* <span data-ttu-id="16886-486">**瀏覽器：** 應用程式正常回應。</span><span class="sxs-lookup"><span data-stu-id="16886-486">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="16886-487">**應用程式記錄檔：** 警告：無法建立 stdoutLogFile \\？\{路徑} \ path_doesnt_exist \ stdout_ {處理序識別碼} _ {TIMESTAMP} .log，ErrorCode =-2147024893。</span><span class="sxs-lookup"><span data-stu-id="16886-487">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="16886-488">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="16886-488">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="16886-489">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-489">Troubleshooting:</span></span>

* <span data-ttu-id="16886-490">`stdoutLogFile`web.config`<aspNetCore>` 中 *元素所指定的* 路徑不存在。</span><span class="sxs-lookup"><span data-stu-id="16886-490">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="16886-491">如需詳細資訊，請參閱[ASP.NET Core 模組：記錄建立和](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)重新導向。</span><span class="sxs-lookup"><span data-stu-id="16886-491">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="16886-492">應用程式集區使用者沒有 stdout 記錄檔路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="16886-492">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="16886-493">應用程式組態一般問題</span><span class="sxs-lookup"><span data-stu-id="16886-493">Application configuration general issue</span></span>

* <span data-ttu-id="16886-494">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="16886-494">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="16886-495">**應用程式記錄檔：** 實體根目錄為 ' C：\{PATH} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} '\' 建立了命令列 ' "C：\{PATH}\{ASSEMBLY} 的進程。{exe | dll} "' 但已當機或沒有回應，或未在指定的埠 ' {PORT} ' 上接聽，ErrorCode = ' {錯誤碼} '</span><span class="sxs-lookup"><span data-stu-id="16886-495">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="16886-496">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="16886-496">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

<span data-ttu-id="16886-497">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="16886-497">Troubleshooting:</span></span>

<span data-ttu-id="16886-498">處理序無法啟動，最可能的原因是應用程式組態或程式設計問題。</span><span class="sxs-lookup"><span data-stu-id="16886-498">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="16886-499">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="16886-499">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>

::: moniker-end
