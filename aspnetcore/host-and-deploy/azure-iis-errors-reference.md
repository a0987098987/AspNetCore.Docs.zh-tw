---
title: Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考
author: guardrex
description: 取得在 Azure Apps Service 與 IIS 上裝載 ASP.NET Core 應用程式時的常見錯誤疑難排解建議。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 3030bc57be113d9034123c96403742442b9240bb
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308095"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="71df1-103">Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考</span><span class="sxs-lookup"><span data-stu-id="71df1-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="71df1-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="71df1-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="71df1-105">本主題提供在 Azure Apps Service 與 IIS 上裝載 ASP.NET Core 應用程式時的常見錯誤疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="71df1-105">This topic offers troubleshooting advice for common errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="71df1-106">收集下列資訊：</span><span class="sxs-lookup"><span data-stu-id="71df1-106">Collect the following information:</span></span>

* <span data-ttu-id="71df1-107">瀏覽器行為 (狀態碼和錯誤訊息)</span><span class="sxs-lookup"><span data-stu-id="71df1-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="71df1-108">應用程式事件記錄檔項目</span><span class="sxs-lookup"><span data-stu-id="71df1-108">Application Event Log entries</span></span>
  * <span data-ttu-id="71df1-109">Azure App Service &ndash; 請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="71df1-109">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="71df1-110">IIS</span><span class="sxs-lookup"><span data-stu-id="71df1-110">IIS</span></span>
    1. <span data-ttu-id="71df1-111">在 [Windows]  功能表中選取 [開始]  ，鍵入「事件檢視器」  ，然後按 **Enter** 鍵。</span><span class="sxs-lookup"><span data-stu-id="71df1-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="71df1-112">在 [事件檢視器]  開啟之後，展開提要欄位中的 [Windows 記錄檔]   > [應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="71df1-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="71df1-113">ASP.NET Core 模組 stdout 和偵錯記錄項目</span><span class="sxs-lookup"><span data-stu-id="71df1-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="71df1-114">Azure App Service &ndash; 請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="71df1-114">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="71df1-115">IIS &ndash; 遵循＜ASP.NET Core 模組＞主題中[記錄檔建立和重新導向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)和[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)小節的指示。</span><span class="sxs-lookup"><span data-stu-id="71df1-115">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="71df1-116">將錯誤資訊與下列常見錯誤進行比較。</span><span class="sxs-lookup"><span data-stu-id="71df1-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="71df1-117">如果找到相符情況，請依照疑難排解建議進行操作。</span><span class="sxs-lookup"><span data-stu-id="71df1-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="71df1-118">本主題中的錯誤清單並不完整。</span><span class="sxs-lookup"><span data-stu-id="71df1-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="71df1-119">如果您遇到這裡未列出的錯誤，請使用本主題底部的 [內容意見反應]  按鈕來開啟新的問題，並提供如何重現錯誤的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="71df1-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="71df1-120">安裝程式無法取得 VC++ 可轉散發套件</span><span class="sxs-lookup"><span data-stu-id="71df1-120">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="71df1-121">**安裝程式例外狀況：** 0x80072efd **--或--** 0x80072f76 - 未指定的錯誤</span><span class="sxs-lookup"><span data-stu-id="71df1-121">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="71df1-122">**安裝程式記錄例外狀況&#8224;：** 錯誤 0x80072efd **--或--** 0x80072f76：無法執行 EXE 套件</span><span class="sxs-lookup"><span data-stu-id="71df1-122">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="71df1-123">&#8224;記錄位於 *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*。</span><span class="sxs-lookup"><span data-stu-id="71df1-123">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="71df1-124">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-124">Troubleshooting:</span></span>

<span data-ttu-id="71df1-125">[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)時，如果系統無法存取網際網路，以致安裝程式無法取得 *Microsoft Visual C++ 2015 可轉散發套件*，就會發生這個例外狀況。</span><span class="sxs-lookup"><span data-stu-id="71df1-125">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="71df1-126">請從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53840)取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="71df1-126">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="71df1-127">如果安裝程式失敗，伺服器可能就無法收到裝載[架構相依部署 (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd) 所需的 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="71df1-127">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="71df1-128">如果要裝載 FDD，請確認已在 [程式和功能]  或 [應用程式與功能]  中安裝執行階段。</span><span class="sxs-lookup"><span data-stu-id="71df1-128">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="71df1-129">如果需要特定執行階段，請從 [.NET Download Archives](https://dotnet.microsoft.com/download/archives) (.NET 下載封存) 下載執行階段，並將它安裝在系統上。</span><span class="sxs-lookup"><span data-stu-id="71df1-129">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="71df1-130">安裝執行階段之後，從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動系統或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="71df1-130">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="71df1-131">作業系統升級已移除 32 位元的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="71df1-131">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="71df1-132">**應用程式記錄檔：** 無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。</span><span class="sxs-lookup"><span data-stu-id="71df1-132">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="71df1-133">資料即錯誤。</span><span class="sxs-lookup"><span data-stu-id="71df1-133">The data is the error.</span></span>

<span data-ttu-id="71df1-134">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-134">Troubleshooting:</span></span>

<span data-ttu-id="71df1-135">作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。</span><span class="sxs-lookup"><span data-stu-id="71df1-135">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="71df1-136">如果先安裝 ASP.NET Core 模組再升級 OS，然後在 OS 升級之後以 32 位元模式執行任何應用程式集區，就會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="71df1-136">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="71df1-137">作業系統升級之後，請修復 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="71df1-137">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="71df1-138">請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="71df1-138">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="71df1-139">執行安裝程式時，請選取 [修復]  。</span><span class="sxs-lookup"><span data-stu-id="71df1-139">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="71df1-140">遺失網站延伸模組、已安裝 32 位元 (x86) 和 64 位元 (x64) 網站延伸模組，或錯誤的處理序位元集合</span><span class="sxs-lookup"><span data-stu-id="71df1-140">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="71df1-141">*適用於 Azure 應用程式服務所裝載的應用程式。*</span><span class="sxs-lookup"><span data-stu-id="71df1-141">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="71df1-142">**瀏覽器：** HTTP 錯誤 500.0 - ANCM 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="71df1-142">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="71df1-143">**應用程式記錄檔：** 叫用 hostfxr 尋找同處理序要求處理常式失敗，不會尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="71df1-143">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="71df1-144">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="71df1-144">Could not find inprocess request handler.</span></span> <span data-ttu-id="71df1-145">已從叫用的 hostfxr 擷取輸出：無法找到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="71df1-145">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="71df1-146">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="71df1-146">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="71df1-147">無法啟動應用程式 '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="71df1-147">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="71df1-148">**ASP.NET Core 模組 stdout 記錄檔：** 無法找到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="71df1-148">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="71df1-149">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="71df1-149">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-150">**ASP.NET Core 模組偵錯記錄：** 叫用 hostfxr 尋找同處理序要求處理常式失敗，不會尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="71df1-150">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="71df1-151">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="71df1-151">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="71df1-152">失敗的 HRESULT 已傳回：0x8000ffff。</span><span class="sxs-lookup"><span data-stu-id="71df1-152">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="71df1-153">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="71df1-153">Could not find inprocess request handler.</span></span> <span data-ttu-id="71df1-154">無法找到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="71df1-154">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="71df1-155">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="71df1-155">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker-end

<span data-ttu-id="71df1-156">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-156">Troubleshooting:</span></span>

* <span data-ttu-id="71df1-157">如果在預覽執行階段中執行應用程式，請安裝與應用程式位元和應用程式執行階段版本相符的 32 位元 (x86) **或** 64 位元 (x64) 網站延伸模組。</span><span class="sxs-lookup"><span data-stu-id="71df1-157">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="71df1-158">**請勿同時安裝這兩個延伸模組或延伸模組的多個執行階段版本。**</span><span class="sxs-lookup"><span data-stu-id="71df1-158">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="71df1-159">ASP.NET Core {RUNTIME VERSION} (x86) 執行階段</span><span class="sxs-lookup"><span data-stu-id="71df1-159">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="71df1-160">ASP.NET Core {RUNTIME VERSION} (x64) 執行階段</span><span class="sxs-lookup"><span data-stu-id="71df1-160">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="71df1-161">重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="71df1-161">Restart the app.</span></span> <span data-ttu-id="71df1-162">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="71df1-162">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="71df1-163">如果在預覽執行階段中執行應用程式，而且已經安裝 32 位元 (x86) 和 64 位元 (x64) [網站延伸模組](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension)，請將與應用程式位元不符的網站延伸模組解除安裝。</span><span class="sxs-lookup"><span data-stu-id="71df1-163">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="71df1-164">移除網站延伸模組之後，請重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="71df1-164">After removing the site extension, restart the app.</span></span> <span data-ttu-id="71df1-165">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="71df1-165">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="71df1-166">如果在預覽執行階段中執行應用程式，且網站延伸模組位元與應用程式相符，請確認預覽網站延伸模組的「執行階段版本」  是否與應用程式相符。</span><span class="sxs-lookup"><span data-stu-id="71df1-166">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="71df1-167">確認**應用程式設定**中應用程式的**平台**與應用程式位元相符。</span><span class="sxs-lookup"><span data-stu-id="71df1-167">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="71df1-168">如需詳細資訊，請參閱 <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>。</span><span class="sxs-lookup"><span data-stu-id="71df1-168">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="71df1-169">已部署 x86 應用程式，但未啟用 32 位元應用程式的應用程式集區</span><span class="sxs-lookup"><span data-stu-id="71df1-169">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="71df1-170">**瀏覽器：** HTTP 錯誤 500.30 - ANCM 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="71df1-170">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="71df1-171">**應用程式記錄檔：** 實體根目錄為 '{PATH}' 的應用程式 '/LM/W3SVC/5/ROOT' 遇到未預期的受控例外狀況，例外狀況代碼 = '0xe0434352'。</span><span class="sxs-lookup"><span data-stu-id="71df1-171">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="71df1-172">如需詳細資訊，請檢查 stderr 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-172">Please check the stderr logs for more information.</span></span> <span data-ttu-id="71df1-173">實體根目錄為 '{PATH}' 的應用程式 '/LM/W3SVC/5/ROOT' 無法載入 CLR 和受控應用程式。</span><span class="sxs-lookup"><span data-stu-id="71df1-173">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="71df1-174">CLR 背景工作執行緒不當結束</span><span class="sxs-lookup"><span data-stu-id="71df1-174">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="71df1-175">**ASP.NET Core 模組 stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="71df1-175">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-176">**ASP.NET Core 模組偵錯記錄：** 失敗的 HRESULT 已傳回：0x8007023e</span><span class="sxs-lookup"><span data-stu-id="71df1-176">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="71df1-177">SDK 會在發行獨立應用程式時攔截到此案例。</span><span class="sxs-lookup"><span data-stu-id="71df1-177">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="71df1-178">如果 RID 不符合平台目標 (例如 `win10-x64` RID 在專案檔中使用 `<PlatformTarget>x86</PlatformTarget>`)，SDK 就會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="71df1-178">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="71df1-179">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-179">Troubleshooting:</span></span>

<span data-ttu-id="71df1-180">針對 x86 架構相依部署 (`<PlatformTarget>x86</PlatformTarget>`)，請啟用 32 位元應用程式的 IIS 應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="71df1-180">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="71df1-181">在 [IIS 管理員] 中，開啟應用程式集區的 [進階設定]  ，然後將 [啟用 32 位元應用程式]  設定為 [True]  。</span><span class="sxs-lookup"><span data-stu-id="71df1-181">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="71df1-182">平台發生 RID 衝突</span><span class="sxs-lookup"><span data-stu-id="71df1-182">Platform conflicts with RID</span></span>

* <span data-ttu-id="71df1-183">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="71df1-183">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="71df1-184">**應用程式記錄檔：** 實體根目錄為 'C:\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 無法使用命令列 '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ' 來啟動處理序，ErrorCode = '0x80004005 : ff。</span><span class="sxs-lookup"><span data-stu-id="71df1-184">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="71df1-185">**ASP.NET Core 模組 stdout 記錄檔：** 未處理的例外狀況：System.BadImageFormatException：無法載入檔案或組件 '{ASSEMBLY}.dll'。</span><span class="sxs-lookup"><span data-stu-id="71df1-185">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="71df1-186">嘗試載入了格式不正確的程式。</span><span class="sxs-lookup"><span data-stu-id="71df1-186">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="71df1-187">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-187">Troubleshooting:</span></span>

* <span data-ttu-id="71df1-188">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="71df1-188">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="71df1-189">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="71df1-189">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="71df1-190">如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="71df1-190">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="71df1-191">如果在升級應用程式和部署更新的組件時，Azure 應用程式部署發生這個例外狀況，請手動刪除來自先前部署的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="71df1-191">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="71df1-192">部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="71df1-192">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="71df1-193">URI 端點錯誤或停止了網站</span><span class="sxs-lookup"><span data-stu-id="71df1-193">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="71df1-194">**瀏覽器：** ERR_CONNECTION_REFUSED **--或--** 無法連線</span><span class="sxs-lookup"><span data-stu-id="71df1-194">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="71df1-195">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="71df1-195">**Application Log:** No entry</span></span>

* <span data-ttu-id="71df1-196">**ASP.NET Core 模組 stdout 記錄檔：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-196">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-197">**ASP.NET Core 模組偵錯記錄：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-197">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="71df1-198">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-198">Troubleshooting:</span></span>

* <span data-ttu-id="71df1-199">確認使用對應用程式正確的 URI 端點。</span><span class="sxs-lookup"><span data-stu-id="71df1-199">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="71df1-200">檢查繫結。</span><span class="sxs-lookup"><span data-stu-id="71df1-200">Check the bindings.</span></span>

* <span data-ttu-id="71df1-201">確認 IIS 網站不是處於「已停止」  狀態。</span><span class="sxs-lookup"><span data-stu-id="71df1-201">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="71df1-202">已停用 CoreWebEngine 或 W3SVC 伺服器功能</span><span class="sxs-lookup"><span data-stu-id="71df1-202">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="71df1-203">**OS 例外狀況：** 必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="71df1-203">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="71df1-204">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-204">Troubleshooting:</span></span>

<span data-ttu-id="71df1-205">確認已啟用適當的角色和功能。</span><span class="sxs-lookup"><span data-stu-id="71df1-205">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="71df1-206">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="71df1-206">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="71df1-207">不正確的網站實體路徑或遺失應用程式</span><span class="sxs-lookup"><span data-stu-id="71df1-207">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="71df1-208">**瀏覽器：** 403 禁止 - 存取被拒 **--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="71df1-208">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="71df1-209">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="71df1-209">**Application Log:** No entry</span></span>

* <span data-ttu-id="71df1-210">**ASP.NET Core 模組 stdout 記錄檔：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-210">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-211">**ASP.NET Core 模組偵錯記錄：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-211">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="71df1-212">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-212">Troubleshooting:</span></span>

<span data-ttu-id="71df1-213">檢查 IIS 網站的 [基本設定]  和實體的應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="71df1-213">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="71df1-214">確認應用程式位於 IIS 網站**實體路徑**的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="71df1-214">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="71df1-215">角色不正確、ASP.NET Core 模組未安裝，或權限不正確</span><span class="sxs-lookup"><span data-stu-id="71df1-215">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="71df1-216">**瀏覽器：** 500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="71df1-216">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="71df1-217">**--或--** 無法顯示此頁面</span><span class="sxs-lookup"><span data-stu-id="71df1-217">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="71df1-218">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="71df1-218">**Application Log:** No entry</span></span>

* <span data-ttu-id="71df1-219">**ASP.NET Core 模組 stdout 記錄檔：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-219">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-220">**ASP.NET Core 模組偵錯記錄：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-220">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="71df1-221">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-221">Troubleshooting:</span></span>

* <span data-ttu-id="71df1-222">確認已啟用適當的角色。</span><span class="sxs-lookup"><span data-stu-id="71df1-222">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="71df1-223">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="71df1-223">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="71df1-224">開啟 [程式和功能]  或 [應用程式與功能]  ，並確認已安裝 **Windows Server Hosting**。</span><span class="sxs-lookup"><span data-stu-id="71df1-224">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="71df1-225">如果已安裝的程式清單中沒有 **Windows Server Hosting**，請下載並安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="71df1-225">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="71df1-226">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="71df1-226">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="71df1-227">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="71df1-227">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="71df1-228">確定 [應用程式集區]   > [處理序模型]   > [身分識別]  已設定為 **ApplicationPoolIdentity**，或自訂身分識別具有正確的權限，可存取應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="71df1-228">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="71df1-229">若解除安裝 ASP.NET Core Hosting Bundle 並安裝舊版裝載套件組合，*applicationHost.config* 檔案不會包括 ASP.NET Core Module 的區段。</span><span class="sxs-lookup"><span data-stu-id="71df1-229">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="71df1-230">開啟位於 *%windir%/System32/inetsrv/config* 的 *applicationHost.config*，然後尋找 `<configuration><configSections><sectionGroup name="system.webServer">` 區段群組。</span><span class="sxs-lookup"><span data-stu-id="71df1-230">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="71df1-231">若區段群組中沒有 ASP.NET Core Module 的區段，請新增區段元素：</span><span class="sxs-lookup"><span data-stu-id="71df1-231">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="71df1-232">或者，您也可以安裝最新版的「ASP.NET Core 裝載套件組合」。</span><span class="sxs-lookup"><span data-stu-id="71df1-232">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="71df1-233">最新版本回溯相容，提供支援的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="71df1-233">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="71df1-234">不正確的 processPath、遺失 PATH 變數、未安裝裝載套件組合、未重新啟動系統/IIS、未安裝 VC++ 可轉散發套件或 dotnet.exe 存取違規</span><span class="sxs-lookup"><span data-stu-id="71df1-234">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-235">**瀏覽器：** HTTP 錯誤 500.0 - ANCM 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="71df1-235">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="71df1-236">**應用程式記錄檔：** 實體根目錄為 'C:\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 無法使用命令列 '"{...}"</span><span class="sxs-lookup"><span data-stu-id="71df1-236">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="71df1-237">' 來啟動處理序，ErrorCode = '0x80070002 :0.</span><span class="sxs-lookup"><span data-stu-id="71df1-237">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="71df1-238">應用程式 '{PATH}' 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="71df1-238">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="71df1-239">在 '{PATH}' 找不到可執行檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-239">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="71df1-240">無法啟動應用程式 '/LM/W3SVC/2/ROOT'，ErrorCode '0x8007023e'。</span><span class="sxs-lookup"><span data-stu-id="71df1-240">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="71df1-241">**ASP.NET Core 模組 stdout 記錄檔：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-241">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="71df1-242">**ASP.NET Core 模組偵錯記錄：** 事件記錄檔：應用程式 '{PATH}' 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="71df1-242">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="71df1-243">在 '{PATH}' 找不到可執行檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-243">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="71df1-244">失敗的 HRESULT 已傳回：0x8007023e</span><span class="sxs-lookup"><span data-stu-id="71df1-244">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="71df1-245">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="71df1-245">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="71df1-246">**應用程式記錄檔：** 實體根目錄為 'C:\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 無法使用命令列 '"{...}"</span><span class="sxs-lookup"><span data-stu-id="71df1-246">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="71df1-247">' 來啟動處理序，ErrorCode = '0x80070002 :0.</span><span class="sxs-lookup"><span data-stu-id="71df1-247">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="71df1-248">**ASP.NET Core 模組 stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="71df1-248">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="71df1-249">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-249">Troubleshooting:</span></span>

* <span data-ttu-id="71df1-250">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="71df1-250">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="71df1-251">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="71df1-251">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="71df1-252">如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="71df1-252">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="71df1-253">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *processPath* 屬性，以確認它是 `dotnet` (適用於架構相依部署 (FDD)) 或 `.\{ASSEMBLY}.exe` (適用於[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd))。</span><span class="sxs-lookup"><span data-stu-id="71df1-253">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="71df1-254">若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="71df1-254">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="71df1-255">確認系統 PATH 設定中有 *C:\Program Files\dotnet\\* 。</span><span class="sxs-lookup"><span data-stu-id="71df1-255">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="71df1-256">若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="71df1-256">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="71df1-257">確認應用程式集區使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。</span><span class="sxs-lookup"><span data-stu-id="71df1-257">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="71df1-258">確認 *C:\Program Files\dotnet* 和應用程式目錄上未針對應用程式集區使用者身分識別設定任何拒絕規則。</span><span class="sxs-lookup"><span data-stu-id="71df1-258">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="71df1-259">可能已部署 FDD 並安裝 .NET Core，但未重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="71df1-259">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="71df1-260">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="71df1-260">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="71df1-261">可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="71df1-261">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="71df1-262">如果尚未安裝 .NET Core 執行階段，請在系統上執行「.NET Core 裝載套件組合安裝程式」  。</span><span class="sxs-lookup"><span data-stu-id="71df1-262">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="71df1-263">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="71df1-263">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="71df1-264">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="71df1-264">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="71df1-265">如果需要特定執行階段，請從 [.NET Download Archives](https://dotnet.microsoft.com/download/archives) (.NET 下載封存) 下載執行階段，並將它安裝在系統上。</span><span class="sxs-lookup"><span data-stu-id="71df1-265">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="71df1-266">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。</span><span class="sxs-lookup"><span data-stu-id="71df1-266">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="71df1-267">可能已部署 FDD，但系統上未安裝 *Microsoft Visual C++ 2015 可轉散發套件 (x64)* 。</span><span class="sxs-lookup"><span data-stu-id="71df1-267">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="71df1-268">請從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53840)取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="71df1-268">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="71df1-269">不正確的 \<aspNetCore> 元素引數</span><span class="sxs-lookup"><span data-stu-id="71df1-269">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-270">**瀏覽器：** HTTP 錯誤 500.0 - ANCM 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="71df1-270">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="71df1-271">**應用程式記錄檔：** 叫用 hostfxr 尋找同處理序要求處理常式失敗，不會尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="71df1-271">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="71df1-272">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="71df1-272">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="71df1-273">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="71df1-273">Could not find inprocess request handler.</span></span> <span data-ttu-id="71df1-274">已從叫用的 hostfxr 擷取輸出：您是不是想執行 dotnet SDK 命令？</span><span class="sxs-lookup"><span data-stu-id="71df1-274">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="71df1-275">請從下列位置安裝 dotnet SDK： https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 。無法啟動應用程式 '/LM/W3SVC/3/ROOT'，ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="71df1-275">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="71df1-276">**ASP.NET Core 模組 stdout 記錄檔：** 您是不是想執行 dotnet SDK 命令？</span><span class="sxs-lookup"><span data-stu-id="71df1-276">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="71df1-277">請從下列位置安裝 dotnet SDK： https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="71df1-277">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="71df1-278">**ASP.NET Core 模組偵錯記錄：** 叫用 hostfxr 尋找同處理序要求處理常式失敗，不會尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="71df1-278">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="71df1-279">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="71df1-279">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="71df1-280">失敗的 HRESULT 已傳回：0x8000ffff 找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="71df1-280">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="71df1-281">已從叫用的 hostfxr 擷取輸出：您是不是想執行 dotnet SDK 命令？</span><span class="sxs-lookup"><span data-stu-id="71df1-281">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="71df1-282">請從下列位置安裝 dotnet SDK： https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 。失敗的 HRESULT 已傳回：0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="71df1-282">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="71df1-283">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="71df1-283">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="71df1-284">**應用程式記錄檔：** 實體根目錄為 'C:\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 無法使用命令列 '"dotnet" .\{ASSEMBLY}.dll' 來啟動處理序，ErrorCode = '0x80004005 :80008081。</span><span class="sxs-lookup"><span data-stu-id="71df1-284">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="71df1-285">**ASP.NET Core 模組 stdout 記錄檔：** 要執行的應用程式不存在：'PATH\{ASSEMBLY}.dll'</span><span class="sxs-lookup"><span data-stu-id="71df1-285">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="71df1-286">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-286">Troubleshooting:</span></span>

* <span data-ttu-id="71df1-287">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="71df1-287">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="71df1-288">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="71df1-288">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="71df1-289">如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="71df1-289">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="71df1-290">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *arguments* 屬性，以確認它是 (a) `.\{ASSEMBLY}.dll` (適用於架構相依部署 (FDD))；或 (b) 不存在、空字串 (`arguments=""`)，或是應用程式的引數清單 (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`，適用於自封式部署 (SCD))。</span><span class="sxs-lookup"><span data-stu-id="71df1-290">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="71df1-291">遺失 .NET Core 共用架構</span><span class="sxs-lookup"><span data-stu-id="71df1-291">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="71df1-292">**瀏覽器：** HTTP 錯誤 500.0 - ANCM 同處理序處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="71df1-292">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="71df1-293">**應用程式記錄檔：** 叫用 hostfxr 尋找同處理序要求處理常式失敗，不會尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="71df1-293">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="71df1-294">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="71df1-294">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="71df1-295">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="71df1-295">Could not find inprocess request handler.</span></span> <span data-ttu-id="71df1-296">已從叫用的 hostfxr 擷取輸出：無法找到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="71df1-296">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="71df1-297">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}'。</span><span class="sxs-lookup"><span data-stu-id="71df1-297">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="71df1-298">無法啟動應用程式 '/LM/W3SVC/5/ROOT'，ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="71df1-298">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="71df1-299">**ASP.NET Core 模組 stdout 記錄檔：** 無法找到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="71df1-299">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="71df1-300">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}'。</span><span class="sxs-lookup"><span data-stu-id="71df1-300">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="71df1-301">**ASP.NET Core 模組偵錯記錄：** 失敗的 HRESULT 已傳回：0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="71df1-301">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="71df1-302">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-302">Troubleshooting:</span></span>

<span data-ttu-id="71df1-303">若為結構相依部署 (FDD)，請確認已在系統上安裝正確的執行階段。</span><span class="sxs-lookup"><span data-stu-id="71df1-303">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="71df1-304">已停止應用程式集區</span><span class="sxs-lookup"><span data-stu-id="71df1-304">Stopped Application Pool</span></span>

* <span data-ttu-id="71df1-305">**瀏覽器：** 503 服務無法使用</span><span class="sxs-lookup"><span data-stu-id="71df1-305">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="71df1-306">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="71df1-306">**Application Log:** No entry</span></span>

* <span data-ttu-id="71df1-307">**ASP.NET Core 模組 stdout 記錄檔：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-307">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-308">**ASP.NET Core 模組偵錯記錄：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-308">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="71df1-309">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-309">Troubleshooting:</span></span>

<span data-ttu-id="71df1-310">確認「應用程式集區」不是處於「已停止」  狀態。</span><span class="sxs-lookup"><span data-stu-id="71df1-310">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="71df1-311">子應用程式包含 \<handlers> 區段</span><span class="sxs-lookup"><span data-stu-id="71df1-311">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="71df1-312">**瀏覽器：** HTTP 錯誤 500.19 - 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="71df1-312">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="71df1-313">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="71df1-313">**Application Log:** No entry</span></span>

* <span data-ttu-id="71df1-314">**ASP.NET Core 模組 stdout 記錄檔：** 根應用程式的記錄檔已建立並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="71df1-314">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="71df1-315">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-315">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-316">**ASP.NET Core 模組偵錯記錄：** 根應用程式的記錄檔已建立並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="71df1-316">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="71df1-317">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-317">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="71df1-318">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-318">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="71df1-319">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段，或子應用程式未繼承父應用程式的處理常式。</span><span class="sxs-lookup"><span data-stu-id="71df1-319">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="71df1-320">父應用程式 *web.config* 的 `<system.webServer>` 區段位於 `<location>` 元素內。</span><span class="sxs-lookup"><span data-stu-id="71df1-320">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="71df1-321">將 <xref:System.Configuration.SectionInformation.InheritInChildApplications*> 屬性設定為 `false`，以表示在 [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 元素內指定的設定，不是由位在父應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="71df1-321">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="71df1-322">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="71df1-322">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="71df1-323">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段。</span><span class="sxs-lookup"><span data-stu-id="71df1-323">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="71df1-324">stdout 記錄檔路徑不正確</span><span class="sxs-lookup"><span data-stu-id="71df1-324">stdout log path incorrect</span></span>

* <span data-ttu-id="71df1-325">**瀏覽器：** 應用程式正常回應。</span><span class="sxs-lookup"><span data-stu-id="71df1-325">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-326">**應用程式記錄檔：** 無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="71df1-326">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="71df1-327">例外狀況訊息：HRESULT 0x80070005 已在 {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84 傳回。</span><span class="sxs-lookup"><span data-stu-id="71df1-327">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="71df1-328">無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中停止 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="71df1-328">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="71df1-329">例外狀況訊息：HRESULT 0x80070002 已在 {PATH} 傳回。</span><span class="sxs-lookup"><span data-stu-id="71df1-329">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="71df1-330">無法在 {PATH}\aspnetcorev2_inprocess.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="71df1-330">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="71df1-331">**ASP.NET Core 模組 stdout 記錄檔：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-331">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="71df1-332">**ASP.NET Core 模組偵錯記錄：** 無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="71df1-332">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="71df1-333">例外狀況訊息：HRESULT 0x80070005 已在 {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84 傳回。</span><span class="sxs-lookup"><span data-stu-id="71df1-333">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="71df1-334">無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中停止 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="71df1-334">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="71df1-335">例外狀況訊息：HRESULT 0x80070002 已在 {PATH} 傳回。</span><span class="sxs-lookup"><span data-stu-id="71df1-335">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="71df1-336">無法在 {PATH}\aspnetcorev2_inprocess.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="71df1-336">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="71df1-337">**應用程式記錄檔：** 警告：無法建立 stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log，ErrorCode = -2147024893。</span><span class="sxs-lookup"><span data-stu-id="71df1-337">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="71df1-338">**ASP.NET Core 模組 stdout 記錄檔：** 未建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-338">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="71df1-339">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-339">Troubleshooting:</span></span>

* <span data-ttu-id="71df1-340">*web.config* 中 `<aspNetCore>` 元素所指定的 `stdoutLogFile` 路徑不存在。</span><span class="sxs-lookup"><span data-stu-id="71df1-340">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="71df1-341">如需詳細資訊，請參閱 [ASP.NET Core 模組：記錄檔建立和重新導向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)。</span><span class="sxs-lookup"><span data-stu-id="71df1-341">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="71df1-342">應用程式集區使用者沒有 stdout 記錄檔路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="71df1-342">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="71df1-343">應用程式組態一般問題</span><span class="sxs-lookup"><span data-stu-id="71df1-343">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="71df1-344">**瀏覽器：** HTTP 錯誤 500.0 - ANCM 同處理序處理常式載入失敗 **--或--** HTTP 錯誤 500.30 - ANCM 同處理序啟動失敗</span><span class="sxs-lookup"><span data-stu-id="71df1-344">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="71df1-345">**應用程式記錄檔：** 變數</span><span class="sxs-lookup"><span data-stu-id="71df1-345">**Application Log:** Variable</span></span>

* <span data-ttu-id="71df1-346">**ASP.NET Core 模組 stdout 記錄檔：** 在應用程式失敗之前，已建立記錄檔，但卻是空的；或已使用一般項目建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="71df1-346">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="71df1-347">**ASP.NET Core 模組偵錯記錄：** 變數</span><span class="sxs-lookup"><span data-stu-id="71df1-347">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="71df1-348">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="71df1-348">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="71df1-349">**應用程式記錄檔：** 實體根目錄為 'C:\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 已使用命令列 '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' 來建立處理序，但可能當機、沒有回應或未在指定的連接埠 '{PORT}' 進行接聽，ErrorCode = '{ERROR CODE}'</span><span class="sxs-lookup"><span data-stu-id="71df1-349">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="71df1-350">**ASP.NET Core 模組 stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="71df1-350">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="71df1-351">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="71df1-351">Troubleshooting:</span></span>

<span data-ttu-id="71df1-352">處理序無法啟動，最可能的原因是應用程式組態或程式設計問題。</span><span class="sxs-lookup"><span data-stu-id="71df1-352">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="71df1-353">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="71df1-353">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>
