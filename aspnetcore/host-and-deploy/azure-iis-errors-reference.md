---
<span data-ttu-id="95788-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="95788-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="95788-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="95788-102">'Blazor'</span></span>
- <span data-ttu-id="95788-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="95788-103">'Identity'</span></span>
- <span data-ttu-id="95788-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="95788-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="95788-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="95788-105">'Razor'</span></span>
- <span data-ttu-id="95788-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="95788-106">'SignalR' uid:</span></span> 

---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="95788-107">Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考</span><span class="sxs-lookup"><span data-stu-id="95788-107">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="95788-108">本主題描述常見的錯誤，並提供在 Azure App Service 和 IIS 上裝載 ASP.NET Core 應用程式時，針對特定錯誤的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="95788-108">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="95788-109">如需一般疑難排解指導方針，請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="95788-109">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="95788-110">收集下列資訊：</span><span class="sxs-lookup"><span data-stu-id="95788-110">Collect the following information:</span></span>

* <span data-ttu-id="95788-111">瀏覽器行為 (狀態碼和錯誤訊息)</span><span class="sxs-lookup"><span data-stu-id="95788-111">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="95788-112">應用程式事件記錄檔項目</span><span class="sxs-lookup"><span data-stu-id="95788-112">Application Event Log entries</span></span>
  * <span data-ttu-id="95788-113">Azure App Service：請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="95788-113">Azure App Service: See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="95788-114">IIS</span><span class="sxs-lookup"><span data-stu-id="95788-114">IIS</span></span>
    1. <span data-ttu-id="95788-115">在 [Windows]\*\*\*\* 功能表中選取 [開始]\*\*\*\*，鍵入「事件檢視器」\*\*，然後按 **Enter** 鍵。</span><span class="sxs-lookup"><span data-stu-id="95788-115">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="95788-116">在 [事件檢視器]\*\*\*\* 開啟之後，展開提要欄位中的 [Windows 記錄檔]**[應用程式]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="95788-116">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="95788-117">ASP.NET Core 模組 stdout 和偵錯記錄項目</span><span class="sxs-lookup"><span data-stu-id="95788-117">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="95788-118">Azure App Service：請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="95788-118">Azure App Service: See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="95788-119">IIS：依照 ASP.NET Core 模組主題中[記錄建立和](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)重新導向和[增強的診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)小節中的指示進行。</span><span class="sxs-lookup"><span data-stu-id="95788-119">IIS: Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="95788-120">將錯誤資訊與下列常見錯誤進行比較。</span><span class="sxs-lookup"><span data-stu-id="95788-120">Compare error information to the following common errors.</span></span> <span data-ttu-id="95788-121">如果找到相符情況，請依照疑難排解建議進行操作。</span><span class="sxs-lookup"><span data-stu-id="95788-121">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="95788-122">本主題中的錯誤清單並不完整。</span><span class="sxs-lookup"><span data-stu-id="95788-122">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="95788-123">如果您遇到這裡未列出的錯誤，請使用本主題底部的 [內容意見反應]\*\*\*\* 按鈕來開啟新的問題，並提供如何重現錯誤的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="95788-123">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="95788-124">作業系統升級已移除 32 位元的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="95788-124">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="95788-125">**應用程式記錄檔：** 無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。</span><span class="sxs-lookup"><span data-stu-id="95788-125">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="95788-126">資料即錯誤。</span><span class="sxs-lookup"><span data-stu-id="95788-126">The data is the error.</span></span>

<span data-ttu-id="95788-127">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-127">Troubleshooting:</span></span>

<span data-ttu-id="95788-128">作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。</span><span class="sxs-lookup"><span data-stu-id="95788-128">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="95788-129">如果先安裝 ASP.NET Core 模組再升級 OS，然後在 OS 升級之後以 32 位元模式執行任何應用程式集區，就會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="95788-129">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="95788-130">作業系統升級之後，請修復 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="95788-130">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="95788-131">請參閱[安裝 .Net Core 裝載](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)套件組合。</span><span class="sxs-lookup"><span data-stu-id="95788-131">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="95788-132">執行安裝程式時，請選取 [修復]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="95788-132">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="95788-133">遺失網站延伸模組、已安裝 32 位元 (x86) 和 64 位元 (x64) 網站延伸模組，或錯誤的處理序位元集合</span><span class="sxs-lookup"><span data-stu-id="95788-133">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="95788-134">*適用於 Azure 應用程式服務所裝載的應用程式。*</span><span class="sxs-lookup"><span data-stu-id="95788-134">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="95788-135">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="95788-135">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="95788-136">**應用程式記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="95788-136">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="95788-137">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="95788-137">Could not find inprocess request handler.</span></span> <span data-ttu-id="95788-138">從叫用用 hostfxr 捕捉的輸出：找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="95788-138">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="95788-139">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="95788-139">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="95788-140">無法啟動應用程式 '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="95788-140">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="95788-141">**ASP.NET Core 模組 Stdout 記錄檔：** 找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="95788-141">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="95788-142">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="95788-142">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

* <span data-ttu-id="95788-143">**ASP.NET Core 模組的 Debug 記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="95788-143">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="95788-144">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="95788-144">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="95788-145">失敗的 HRESULT 傳回：0x8000ffff。</span><span class="sxs-lookup"><span data-stu-id="95788-145">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="95788-146">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="95788-146">Could not find inprocess request handler.</span></span> <span data-ttu-id="95788-147">無法找到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="95788-147">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="95788-148">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="95788-148">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

<span data-ttu-id="95788-149">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-149">Troubleshooting:</span></span>

* <span data-ttu-id="95788-150">如果在預覽執行階段中執行應用程式，請安裝與應用程式位元和應用程式執行階段版本相符的 32 位元 (x86) **或** 64 位元 (x64) 網站延伸模組。</span><span class="sxs-lookup"><span data-stu-id="95788-150">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="95788-151">**請勿同時安裝這兩個延伸模組或延伸模組的多個執行階段版本。**</span><span class="sxs-lookup"><span data-stu-id="95788-151">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="95788-152">ASP.NET Core {RUNTIME VERSION} (x86) 執行階段</span><span class="sxs-lookup"><span data-stu-id="95788-152">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="95788-153">ASP.NET Core {RUNTIME VERSION} (x64) 執行階段</span><span class="sxs-lookup"><span data-stu-id="95788-153">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="95788-154">重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="95788-154">Restart the app.</span></span> <span data-ttu-id="95788-155">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="95788-155">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="95788-156">如果在預覽執行階段中執行應用程式，而且已經安裝 32 位元 (x86) 和 64 位元 (x64) [網站延伸模組](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension)，請將與應用程式位元不符的網站延伸模組解除安裝。</span><span class="sxs-lookup"><span data-stu-id="95788-156">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="95788-157">移除網站延伸模組之後，請重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="95788-157">After removing the site extension, restart the app.</span></span> <span data-ttu-id="95788-158">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="95788-158">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="95788-159">如果在預覽執行階段中執行應用程式，且網站延伸模組位元與應用程式相符，請確認預覽網站延伸模組的「執行階段版本」\*\* 是否與應用程式相符。</span><span class="sxs-lookup"><span data-stu-id="95788-159">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="95788-160">確認**應用程式設定**中應用程式的**平台**與應用程式位元相符。</span><span class="sxs-lookup"><span data-stu-id="95788-160">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="95788-161">如需詳細資訊，請參閱<xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>。</span><span class="sxs-lookup"><span data-stu-id="95788-161">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="95788-162">已部署 x86 應用程式，但未啟用 32 位元應用程式的應用程式集區</span><span class="sxs-lookup"><span data-stu-id="95788-162">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="95788-163">**瀏覽器：** HTTP 錯誤 500.30-ANCM 同進程啟動失敗</span><span class="sxs-lookup"><span data-stu-id="95788-163">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="95788-164">**應用程式記錄檔：** 實體根 ' {PATH} ' 的應用程式 '/LM/W3SVC/5/ROOT ' 遇到非預期的 managed 例外狀況，例外狀況代碼 = ' 0xe0434352 '。</span><span class="sxs-lookup"><span data-stu-id="95788-164">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="95788-165">如需詳細資訊，請檢查 stderr 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-165">Please check the stderr logs for more information.</span></span> <span data-ttu-id="95788-166">實體根目錄為 '{PATH}' 的應用程式 '/LM/W3SVC/5/ROOT' 無法載入 CLR 和受控應用程式。</span><span class="sxs-lookup"><span data-stu-id="95788-166">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="95788-167">CLR 背景工作執行緒不當結束</span><span class="sxs-lookup"><span data-stu-id="95788-167">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="95788-168">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="95788-168">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

* <span data-ttu-id="95788-169">**ASP.NET Core 模組的 Debug 記錄檔：** 失敗的 HRESULT 傳回：0x8007023e</span><span class="sxs-lookup"><span data-stu-id="95788-169">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

<span data-ttu-id="95788-170">SDK 會在發行獨立應用程式時攔截到此案例。</span><span class="sxs-lookup"><span data-stu-id="95788-170">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="95788-171">如果 RID 不符合平台目標 (例如 `win10-x64` RID 在專案檔中使用 `<PlatformTarget>x86</PlatformTarget>`)，SDK 就會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="95788-171">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="95788-172">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-172">Troubleshooting:</span></span>

<span data-ttu-id="95788-173">針對 x86 架構相依部署 (`<PlatformTarget>x86</PlatformTarget>`)，請啟用 32 位元應用程式的 IIS 應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="95788-173">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="95788-174">在 [IIS 管理員] 中，開啟應用程式集區的 [進階設定]\*\*\*\*，然後將 [啟用 32 位元應用程式]\*\*\*\* 設定為 [True]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="95788-174">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="95788-175">平台發生 RID 衝突</span><span class="sxs-lookup"><span data-stu-id="95788-175">Platform conflicts with RID</span></span>

* <span data-ttu-id="95788-176">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="95788-176">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="95788-177">**應用程式記錄檔：** 實體根目錄為 ' C： path} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' \{ \' 無法以命令列 ' "C： \{ PATH} {ASSEMBLY} 啟動進程。 {exe | dll} "'，ErrorCode = ' 0x80004005： ff。</span><span class="sxs-lookup"><span data-stu-id="95788-177">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="95788-178">**ASP.NET Core 模組 Stdout 記錄檔：** 未處理的例外狀況： System.badimageformatexception>：無法載入檔案或元件 ' {ASSEMBLY} .dll '。</span><span class="sxs-lookup"><span data-stu-id="95788-178">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="95788-179">嘗試載入了格式不正確的程式。</span><span class="sxs-lookup"><span data-stu-id="95788-179">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="95788-180">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-180">Troubleshooting:</span></span>

* <span data-ttu-id="95788-181">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="95788-181">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="95788-182">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="95788-182">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="95788-183">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="95788-183">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="95788-184">如果在升級應用程式和部署更新的組件時，Azure 應用程式部署發生這個例外狀況，請手動刪除來自先前部署的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="95788-184">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="95788-185">部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="95788-185">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="95788-186">URI 端點錯誤或停止了網站</span><span class="sxs-lookup"><span data-stu-id="95788-186">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="95788-187">**瀏覽器：** ERR_CONNECTION_REFUSED **--或--** 無法連接</span><span class="sxs-lookup"><span data-stu-id="95788-187">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="95788-188">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="95788-188">**Application Log:** No entry</span></span>

* <span data-ttu-id="95788-189">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-189">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="95788-190">**ASP.NET Core 模組的 Debug 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-190">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="95788-191">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-191">Troubleshooting:</span></span>

* <span data-ttu-id="95788-192">確認使用對應用程式正確的 URI 端點。</span><span class="sxs-lookup"><span data-stu-id="95788-192">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="95788-193">檢查繫結。</span><span class="sxs-lookup"><span data-stu-id="95788-193">Check the bindings.</span></span>

* <span data-ttu-id="95788-194">確認 IIS 網站不是處於「已停止」\*\* 狀態。</span><span class="sxs-lookup"><span data-stu-id="95788-194">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="95788-195">已停用 CoreWebEngine 或 W3SVC 伺服器功能</span><span class="sxs-lookup"><span data-stu-id="95788-195">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="95788-196">**作業系統例外狀況：** 必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="95788-196">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="95788-197">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-197">Troubleshooting:</span></span>

<span data-ttu-id="95788-198">確認已啟用適當的角色和功能。</span><span class="sxs-lookup"><span data-stu-id="95788-198">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="95788-199">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="95788-199">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="95788-200">不正確的網站實體路徑或遺失應用程式</span><span class="sxs-lookup"><span data-stu-id="95788-200">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="95788-201">**瀏覽器：** 403 禁止 - 存取被拒 **--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="95788-201">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="95788-202">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="95788-202">**Application Log:** No entry</span></span>

* <span data-ttu-id="95788-203">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-203">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="95788-204">**ASP.NET Core 模組的 Debug 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-204">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="95788-205">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-205">Troubleshooting:</span></span>

<span data-ttu-id="95788-206">檢查 IIS 網站的 [基本設定]\*\*\*\* 和實體的應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="95788-206">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="95788-207">確認應用程式位於 IIS 網站**實體路徑**的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="95788-207">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="95788-208">角色不正確、ASP.NET Core 模組未安裝，或權限不正確</span><span class="sxs-lookup"><span data-stu-id="95788-208">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="95788-209">**瀏覽器：** 500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="95788-209">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="95788-210">**--或--** 無法顯示此頁面</span><span class="sxs-lookup"><span data-stu-id="95788-210">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="95788-211">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="95788-211">**Application Log:** No entry</span></span>

* <span data-ttu-id="95788-212">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-212">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="95788-213">**ASP.NET Core 模組的 Debug 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-213">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="95788-214">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-214">Troubleshooting:</span></span>

* <span data-ttu-id="95788-215">確認已啟用適當的角色。</span><span class="sxs-lookup"><span data-stu-id="95788-215">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="95788-216">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="95788-216">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="95788-217">開啟 [程式和功能]\*\*\*\* 或 [應用程式與功能]\*\*\*\*，並確認已安裝 **Windows Server Hosting**。</span><span class="sxs-lookup"><span data-stu-id="95788-217">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="95788-218">如果已安裝的程式清單中沒有 **Windows Server Hosting**，請下載並安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="95788-218">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="95788-219">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="95788-219">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="95788-220">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="95788-220">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="95788-221">請確定**應用程式**集區 > **進程模型** > **Identity** 已設定為**ApplicationPoolIdentity** ，或自訂身分識別具有正確的許可權，可存取應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="95788-221">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="95788-222">若解除安裝 ASP.NET Core Hosting Bundle 並安裝舊版裝載套件組合，*applicationHost.config* 檔案不會包括 ASP.NET Core Module 的區段。</span><span class="sxs-lookup"><span data-stu-id="95788-222">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="95788-223">開啟位於 *%windir%/System32/inetsrv/config* 的 *applicationHost.config*，然後尋找 `<configuration><configSections><sectionGroup name="system.webServer">` 區段群組。</span><span class="sxs-lookup"><span data-stu-id="95788-223">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="95788-224">若區段群組中沒有 ASP.NET Core Module 的區段，請新增區段元素：</span><span class="sxs-lookup"><span data-stu-id="95788-224">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="95788-225">或者，您也可以安裝最新版的「ASP.NET Core 裝載套件組合」。</span><span class="sxs-lookup"><span data-stu-id="95788-225">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="95788-226">最新版本回溯相容，提供支援的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="95788-226">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="95788-227">不正確的 processPath、遺失 PATH 變數、未安裝裝載套件組合、未重新啟動系統/IIS、未安裝 VC++ 可轉散發套件或 dotnet.exe 存取違規</span><span class="sxs-lookup"><span data-stu-id="95788-227">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="95788-228">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="95788-228">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="95788-229">**應用程式記錄檔：** 實體根目錄為 ' C： PATH} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' \{ \' 無法使用命令列 ' "{...}" 來啟動進程</span><span class="sxs-lookup"><span data-stu-id="95788-229">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="95788-230">'，ErrorCode = ' 0x80070002：0。</span><span class="sxs-lookup"><span data-stu-id="95788-230">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="95788-231">應用程式 '{PATH}' 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="95788-231">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="95788-232">在 '{PATH}' 找不到可執行檔。</span><span class="sxs-lookup"><span data-stu-id="95788-232">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="95788-233">無法啟動應用程式 '/LM/W3SVC/2/ROOT'，ErrorCode '0x8007023e'。</span><span class="sxs-lookup"><span data-stu-id="95788-233">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="95788-234">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-234">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="95788-235">**ASP.NET Core 模組的 Debug 記錄檔：** 事件記錄檔： ' 應用程式 ' {PATH} ' 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="95788-235">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="95788-236">在 '{PATH}' 找不到可執行檔。</span><span class="sxs-lookup"><span data-stu-id="95788-236">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="95788-237">失敗的 HRESULT 傳回：0x8007023e</span><span class="sxs-lookup"><span data-stu-id="95788-237">Failed HRESULT returned: 0x8007023e</span></span>

<span data-ttu-id="95788-238">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-238">Troubleshooting:</span></span>

* <span data-ttu-id="95788-239">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="95788-239">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="95788-240">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="95788-240">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="95788-241">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="95788-241">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="95788-242">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *processPath* 屬性，以確認它是 `dotnet` (適用於架構相依部署 (FDD)) 或 `.\{ASSEMBLY}.exe` (適用於[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd))。</span><span class="sxs-lookup"><span data-stu-id="95788-242">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="95788-243">若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="95788-243">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="95788-244">確認\*C:\Program Files\dotnet \\ \*存在於系統路徑設定中。</span><span class="sxs-lookup"><span data-stu-id="95788-244">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="95788-245">若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="95788-245">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="95788-246">確認應用程式集區使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。</span><span class="sxs-lookup"><span data-stu-id="95788-246">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="95788-247">確認 *C:\Program Files\dotnet* 和應用程式目錄上未針對應用程式集區使用者身分識別設定任何拒絕規則。</span><span class="sxs-lookup"><span data-stu-id="95788-247">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="95788-248">可能已部署 FDD 並安裝 .NET Core，但未重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="95788-248">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="95788-249">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="95788-249">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="95788-250">可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="95788-250">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="95788-251">如果尚未安裝 .NET Core 執行階段，請在系統上執行「.NET Core 裝載套件組合安裝程式」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="95788-251">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="95788-252">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="95788-252">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="95788-253">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="95788-253">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="95788-254">如果需要特定執行階段，請從 [.NET Download Archives](https://dotnet.microsoft.com/download/archives) (.NET 下載封存) 下載執行階段，並將它安裝在系統上。</span><span class="sxs-lookup"><span data-stu-id="95788-254">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="95788-255">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。</span><span class="sxs-lookup"><span data-stu-id="95788-255">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="95788-256">元素的引數不正確 \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="95788-256">Incorrect arguments of \<aspNetCore> element</span></span>

* <span data-ttu-id="95788-257">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="95788-257">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="95788-258">**應用程式記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="95788-258">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="95788-259">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="95788-259">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="95788-260">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="95788-260">Could not find inprocess request handler.</span></span> <span data-ttu-id="95788-261">從叫用用 hostfxr 捕獲的輸出：您是否說要執行 dotnet SDK 命令？</span><span class="sxs-lookup"><span data-stu-id="95788-261">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="95788-262">請從下列安裝 dotnet SDK： https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 無法啟動應用程式 '/LM/W3SVC/3/ROOT '，ErrorCode ' 0x8000ffff '。</span><span class="sxs-lookup"><span data-stu-id="95788-262">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="95788-263">**ASP.NET Core 模組 Stdout 記錄檔：** 您是否表示要執行 dotnet SDK 命令？</span><span class="sxs-lookup"><span data-stu-id="95788-263">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="95788-264">請從下列位置安裝 dotnet SDK：https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="95788-264">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="95788-265">**ASP.NET Core 模組的 Debug 記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="95788-265">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="95788-266">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="95788-266">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="95788-267">失敗的 HRESULT 傳回：0x8000ffff 找不到進程間要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="95788-267">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="95788-268">從叫用用 hostfxr 捕獲的輸出：您是否說要執行 dotnet SDK 命令？</span><span class="sxs-lookup"><span data-stu-id="95788-268">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="95788-269">請從下列安裝 dotnet SDK： https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 FAILED HRESULT 傳回：0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="95788-269">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

<span data-ttu-id="95788-270">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-270">Troubleshooting:</span></span>

* <span data-ttu-id="95788-271">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="95788-271">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="95788-272">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="95788-272">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="95788-273">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="95788-273">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="95788-274">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *arguments* 屬性，以確認它是 (a) `.\{ASSEMBLY}.dll` (適用於架構相依部署 (FDD))；或 (b) 不存在、空字串 (`arguments=""`)，或是應用程式的引數清單 (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`，適用於自封式部署 (SCD))。</span><span class="sxs-lookup"><span data-stu-id="95788-274">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="95788-275">遺失 .NET Core 共用架構</span><span class="sxs-lookup"><span data-stu-id="95788-275">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="95788-276">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="95788-276">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="95788-277">**應用程式記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="95788-277">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="95788-278">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="95788-278">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="95788-279">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="95788-279">Could not find inprocess request handler.</span></span> <span data-ttu-id="95788-280">從叫用用 hostfxr 捕捉的輸出：找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="95788-280">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="95788-281">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}'。</span><span class="sxs-lookup"><span data-stu-id="95788-281">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="95788-282">無法啟動應用程式 '/LM/W3SVC/5/ROOT'，ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="95788-282">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="95788-283">**ASP.NET Core 模組 Stdout 記錄檔：** 找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="95788-283">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="95788-284">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}'。</span><span class="sxs-lookup"><span data-stu-id="95788-284">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="95788-285">**ASP.NET Core 模組的 Debug 記錄檔：** 失敗的 HRESULT 傳回：0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="95788-285">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

<span data-ttu-id="95788-286">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-286">Troubleshooting:</span></span>

<span data-ttu-id="95788-287">若為結構相依部署 (FDD)，請確認已在系統上安裝正確的執行階段。</span><span class="sxs-lookup"><span data-stu-id="95788-287">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="95788-288">已停止應用程式集區</span><span class="sxs-lookup"><span data-stu-id="95788-288">Stopped Application Pool</span></span>

* <span data-ttu-id="95788-289">**瀏覽器：** 503 服務無法使用</span><span class="sxs-lookup"><span data-stu-id="95788-289">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="95788-290">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="95788-290">**Application Log:** No entry</span></span>

* <span data-ttu-id="95788-291">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-291">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="95788-292">**ASP.NET Core 模組的 Debug 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-292">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="95788-293">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-293">Troubleshooting:</span></span>

<span data-ttu-id="95788-294">確認「應用程式集區」不是處於「已停止」\*\* 狀態。</span><span class="sxs-lookup"><span data-stu-id="95788-294">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="95788-295">子應用程式包含 \<handlers> 區段</span><span class="sxs-lookup"><span data-stu-id="95788-295">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="95788-296">**瀏覽器：** HTTP 錯誤 500.19 - 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="95788-296">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="95788-297">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="95788-297">**Application Log:** No entry</span></span>

* <span data-ttu-id="95788-298">**ASP.NET Core 模組 Stdout 記錄檔：** 根應用程式的記錄檔隨即建立，並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="95788-298">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="95788-299">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-299">The sub-app's log file isn't created.</span></span>

* <span data-ttu-id="95788-300">**ASP.NET Core 模組的 Debug 記錄檔：** 根應用程式的記錄檔隨即建立，並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="95788-300">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="95788-301">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-301">The sub-app's log file isn't created.</span></span>

<span data-ttu-id="95788-302">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-302">Troubleshooting:</span></span>

<span data-ttu-id="95788-303">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段，或子應用程式未繼承父應用程式的處理常式。</span><span class="sxs-lookup"><span data-stu-id="95788-303">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="95788-304">父應用程式 *web.config* 的 `<system.webServer>` 區段位於 `<location>` 元素內。</span><span class="sxs-lookup"><span data-stu-id="95788-304">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="95788-305"><xref:System.Configuration.SectionInformation.InheritInChildApplications*>屬性會設定為 `false` ，表示在專案內指定的設定 [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 不會由位於父系應用程式子目錄中的應用程式繼承。</span><span class="sxs-lookup"><span data-stu-id="95788-305">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="95788-306">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="95788-306">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="95788-307">stdout 記錄檔路徑不正確</span><span class="sxs-lookup"><span data-stu-id="95788-307">stdout log path incorrect</span></span>

* <span data-ttu-id="95788-308">**瀏覽器：** 應用程式正常回應。</span><span class="sxs-lookup"><span data-stu-id="95788-308">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="95788-309">**應用程式記錄檔：** 無法在 C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll 中啟動 stdout 重新導向</span><span class="sxs-lookup"><span data-stu-id="95788-309">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="95788-310">例外狀況訊息：已在 {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp：84傳回 HRESULT 0x80070005。</span><span class="sxs-lookup"><span data-stu-id="95788-310">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="95788-311">無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中停止 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="95788-311">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="95788-312">例外狀況訊息：在 {PATH} 傳回 HRESULT 0x80070002。</span><span class="sxs-lookup"><span data-stu-id="95788-312">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="95788-313">無法在 {PATH}\aspnetcorev2_inprocess.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="95788-313">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="95788-314">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-314">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="95788-315">**ASP.NET Core 模組的 Debug 記錄檔：** 無法在 C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll 中啟動 stdout 重新導向</span><span class="sxs-lookup"><span data-stu-id="95788-315">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="95788-316">例外狀況訊息：已在 {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp：84傳回 HRESULT 0x80070005。</span><span class="sxs-lookup"><span data-stu-id="95788-316">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="95788-317">無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中停止 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="95788-317">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="95788-318">例外狀況訊息：在 {PATH} 傳回 HRESULT 0x80070002。</span><span class="sxs-lookup"><span data-stu-id="95788-318">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="95788-319">無法在 {PATH}\aspnetcorev2_inprocess.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="95788-319">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

<span data-ttu-id="95788-320">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-320">Troubleshooting:</span></span>

* <span data-ttu-id="95788-321">*web.config* 中 `<aspNetCore>` 元素所指定的 `stdoutLogFile` 路徑不存在。</span><span class="sxs-lookup"><span data-stu-id="95788-321">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="95788-322">如需詳細資訊，請參閱[ASP.NET Core 模組：記錄建立和](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)重新導向。</span><span class="sxs-lookup"><span data-stu-id="95788-322">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="95788-323">應用程式集區使用者沒有 stdout 記錄檔路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="95788-323">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="95788-324">應用程式組態一般問題</span><span class="sxs-lookup"><span data-stu-id="95788-324">Application configuration general issue</span></span>

* <span data-ttu-id="95788-325">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗 **--或--** HTTP 錯誤 500.30-ANCM 同進程啟動失敗</span><span class="sxs-lookup"><span data-stu-id="95788-325">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="95788-326">**應用程式記錄檔：** 變</span><span class="sxs-lookup"><span data-stu-id="95788-326">**Application Log:** Variable</span></span>

* <span data-ttu-id="95788-327">**ASP.NET Core 模組 Stdout 記錄檔：** 在應用程式失敗之前，會建立記錄檔，但卻是空的或以一般專案建立的。</span><span class="sxs-lookup"><span data-stu-id="95788-327">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="95788-328">**ASP.NET Core 模組的 Debug 記錄檔：** 變</span><span class="sxs-lookup"><span data-stu-id="95788-328">**ASP.NET Core Module Debug Log:** Variable</span></span>

<span data-ttu-id="95788-329">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-329">Troubleshooting:</span></span>

<span data-ttu-id="95788-330">處理序無法啟動，最可能的原因是應用程式組態或程式設計問題。</span><span class="sxs-lookup"><span data-stu-id="95788-330">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="95788-331">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="95788-331">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="95788-332">本主題描述常見的錯誤，並提供在 Azure App Service 和 IIS 上裝載 ASP.NET Core 應用程式時，針對特定錯誤的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="95788-332">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="95788-333">如需一般疑難排解指導方針，請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="95788-333">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="95788-334">收集下列資訊：</span><span class="sxs-lookup"><span data-stu-id="95788-334">Collect the following information:</span></span>

* <span data-ttu-id="95788-335">瀏覽器行為 (狀態碼和錯誤訊息)</span><span class="sxs-lookup"><span data-stu-id="95788-335">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="95788-336">應用程式事件記錄檔項目</span><span class="sxs-lookup"><span data-stu-id="95788-336">Application Event Log entries</span></span>
  * <span data-ttu-id="95788-337">Azure App Service：請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="95788-337">Azure App Service: See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="95788-338">IIS</span><span class="sxs-lookup"><span data-stu-id="95788-338">IIS</span></span>
    1. <span data-ttu-id="95788-339">在 [Windows]\*\*\*\* 功能表中選取 [開始]\*\*\*\*，鍵入「事件檢視器」\*\*，然後按 **Enter** 鍵。</span><span class="sxs-lookup"><span data-stu-id="95788-339">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="95788-340">在 [事件檢視器]\*\*\*\* 開啟之後，展開提要欄位中的 [Windows 記錄檔]**[應用程式]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="95788-340">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="95788-341">ASP.NET Core 模組 stdout 和偵錯記錄項目</span><span class="sxs-lookup"><span data-stu-id="95788-341">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="95788-342">Azure App Service：請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="95788-342">Azure App Service: See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="95788-343">IIS：依照 ASP.NET Core 模組主題中[記錄建立和](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)重新導向和[增強的診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)小節中的指示進行。</span><span class="sxs-lookup"><span data-stu-id="95788-343">IIS: Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="95788-344">將錯誤資訊與下列常見錯誤進行比較。</span><span class="sxs-lookup"><span data-stu-id="95788-344">Compare error information to the following common errors.</span></span> <span data-ttu-id="95788-345">如果找到相符情況，請依照疑難排解建議進行操作。</span><span class="sxs-lookup"><span data-stu-id="95788-345">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="95788-346">本主題中的錯誤清單並不完整。</span><span class="sxs-lookup"><span data-stu-id="95788-346">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="95788-347">如果您遇到這裡未列出的錯誤，請使用本主題底部的 [內容意見反應]\*\*\*\* 按鈕來開啟新的問題，並提供如何重現錯誤的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="95788-347">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="95788-348">作業系統升級已移除 32 位元的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="95788-348">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="95788-349">**應用程式記錄檔：** 無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。</span><span class="sxs-lookup"><span data-stu-id="95788-349">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="95788-350">資料即錯誤。</span><span class="sxs-lookup"><span data-stu-id="95788-350">The data is the error.</span></span>

<span data-ttu-id="95788-351">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-351">Troubleshooting:</span></span>

<span data-ttu-id="95788-352">作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。</span><span class="sxs-lookup"><span data-stu-id="95788-352">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="95788-353">如果先安裝 ASP.NET Core 模組再升級 OS，然後在 OS 升級之後以 32 位元模式執行任何應用程式集區，就會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="95788-353">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="95788-354">作業系統升級之後，請修復 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="95788-354">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="95788-355">請參閱[安裝 .Net Core 裝載](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)套件組合。</span><span class="sxs-lookup"><span data-stu-id="95788-355">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="95788-356">執行安裝程式時，請選取 [修復]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="95788-356">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="95788-357">遺失網站延伸模組、已安裝 32 位元 (x86) 和 64 位元 (x64) 網站延伸模組，或錯誤的處理序位元集合</span><span class="sxs-lookup"><span data-stu-id="95788-357">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="95788-358">*適用於 Azure 應用程式服務所裝載的應用程式。*</span><span class="sxs-lookup"><span data-stu-id="95788-358">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="95788-359">**瀏覽器：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="95788-359">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="95788-360">**應用程式記錄檔：** 叫用用 hostfxr 以尋找進程中的要求處理常式失敗，而不需要尋找任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="95788-360">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="95788-361">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="95788-361">Could not find inprocess request handler.</span></span> <span data-ttu-id="95788-362">從叫用用 hostfxr 捕捉的輸出：找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="95788-362">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="95788-363">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="95788-363">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="95788-364">無法啟動應用程式 '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="95788-364">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="95788-365">**ASP.NET Core 模組 Stdout 記錄檔：** 找不到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="95788-365">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="95788-366">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="95788-366">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

<span data-ttu-id="95788-367">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-367">Troubleshooting:</span></span>

* <span data-ttu-id="95788-368">如果在預覽執行階段中執行應用程式，請安裝與應用程式位元和應用程式執行階段版本相符的 32 位元 (x86) **或** 64 位元 (x64) 網站延伸模組。</span><span class="sxs-lookup"><span data-stu-id="95788-368">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="95788-369">**請勿同時安裝這兩個延伸模組或延伸模組的多個執行階段版本。**</span><span class="sxs-lookup"><span data-stu-id="95788-369">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="95788-370">ASP.NET Core {RUNTIME VERSION} (x86) 執行階段</span><span class="sxs-lookup"><span data-stu-id="95788-370">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="95788-371">ASP.NET Core {RUNTIME VERSION} (x64) 執行階段</span><span class="sxs-lookup"><span data-stu-id="95788-371">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="95788-372">重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="95788-372">Restart the app.</span></span> <span data-ttu-id="95788-373">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="95788-373">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="95788-374">如果在預覽執行階段中執行應用程式，而且已經安裝 32 位元 (x86) 和 64 位元 (x64) [網站延伸模組](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension)，請將與應用程式位元不符的網站延伸模組解除安裝。</span><span class="sxs-lookup"><span data-stu-id="95788-374">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="95788-375">移除網站延伸模組之後，請重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="95788-375">After removing the site extension, restart the app.</span></span> <span data-ttu-id="95788-376">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="95788-376">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="95788-377">如果在預覽執行階段中執行應用程式，且網站延伸模組位元與應用程式相符，請確認預覽網站延伸模組的「執行階段版本」\*\* 是否與應用程式相符。</span><span class="sxs-lookup"><span data-stu-id="95788-377">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="95788-378">確認**應用程式設定**中應用程式的**平台**與應用程式位元相符。</span><span class="sxs-lookup"><span data-stu-id="95788-378">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="95788-379">如需詳細資訊，請參閱<xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>。</span><span class="sxs-lookup"><span data-stu-id="95788-379">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="95788-380">已部署 x86 應用程式，但未啟用 32 位元應用程式的應用程式集區</span><span class="sxs-lookup"><span data-stu-id="95788-380">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="95788-381">**瀏覽器：** HTTP 錯誤 500.30-ANCM 同進程啟動失敗</span><span class="sxs-lookup"><span data-stu-id="95788-381">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="95788-382">**應用程式記錄檔：** 實體根 ' {PATH} ' 的應用程式 '/LM/W3SVC/5/ROOT ' 遇到非預期的 managed 例外狀況，例外狀況代碼 = ' 0xe0434352 '。</span><span class="sxs-lookup"><span data-stu-id="95788-382">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="95788-383">如需詳細資訊，請檢查 stderr 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-383">Please check the stderr logs for more information.</span></span> <span data-ttu-id="95788-384">實體根目錄為 '{PATH}' 的應用程式 '/LM/W3SVC/5/ROOT' 無法載入 CLR 和受控應用程式。</span><span class="sxs-lookup"><span data-stu-id="95788-384">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="95788-385">CLR 背景工作執行緒不當結束</span><span class="sxs-lookup"><span data-stu-id="95788-385">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="95788-386">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="95788-386">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

<span data-ttu-id="95788-387">SDK 會在發行獨立應用程式時攔截到此案例。</span><span class="sxs-lookup"><span data-stu-id="95788-387">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="95788-388">如果 RID 不符合平台目標 (例如 `win10-x64` RID 在專案檔中使用 `<PlatformTarget>x86</PlatformTarget>`)，SDK 就會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="95788-388">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="95788-389">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-389">Troubleshooting:</span></span>

<span data-ttu-id="95788-390">針對 x86 架構相依部署 (`<PlatformTarget>x86</PlatformTarget>`)，請啟用 32 位元應用程式的 IIS 應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="95788-390">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="95788-391">在 [IIS 管理員] 中，開啟應用程式集區的 [進階設定]\*\*\*\*，然後將 [啟用 32 位元應用程式]\*\*\*\* 設定為 [True]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="95788-391">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="95788-392">平台發生 RID 衝突</span><span class="sxs-lookup"><span data-stu-id="95788-392">Platform conflicts with RID</span></span>

* <span data-ttu-id="95788-393">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="95788-393">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="95788-394">**應用程式記錄檔：** 實體根目錄為 ' C： path} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' \{ \' 無法以命令列 ' "C： \{ PATH} {ASSEMBLY} 啟動進程。 {exe | dll} "'，ErrorCode = ' 0x80004005： ff。</span><span class="sxs-lookup"><span data-stu-id="95788-394">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="95788-395">**ASP.NET Core 模組 Stdout 記錄檔：** 未處理的例外狀況： System.badimageformatexception>：無法載入檔案或元件 ' {ASSEMBLY} .dll '。</span><span class="sxs-lookup"><span data-stu-id="95788-395">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="95788-396">嘗試載入了格式不正確的程式。</span><span class="sxs-lookup"><span data-stu-id="95788-396">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="95788-397">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-397">Troubleshooting:</span></span>

* <span data-ttu-id="95788-398">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="95788-398">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="95788-399">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="95788-399">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="95788-400">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="95788-400">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="95788-401">如果在升級應用程式和部署更新的組件時，Azure 應用程式部署發生這個例外狀況，請手動刪除來自先前部署的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="95788-401">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="95788-402">部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="95788-402">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="95788-403">URI 端點錯誤或停止了網站</span><span class="sxs-lookup"><span data-stu-id="95788-403">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="95788-404">**瀏覽器：** ERR_CONNECTION_REFUSED **--或--** 無法連接</span><span class="sxs-lookup"><span data-stu-id="95788-404">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="95788-405">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="95788-405">**Application Log:** No entry</span></span>

* <span data-ttu-id="95788-406">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-406">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="95788-407">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-407">Troubleshooting:</span></span>

* <span data-ttu-id="95788-408">確認使用對應用程式正確的 URI 端點。</span><span class="sxs-lookup"><span data-stu-id="95788-408">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="95788-409">檢查繫結。</span><span class="sxs-lookup"><span data-stu-id="95788-409">Check the bindings.</span></span>

* <span data-ttu-id="95788-410">確認 IIS 網站不是處於「已停止」\*\* 狀態。</span><span class="sxs-lookup"><span data-stu-id="95788-410">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="95788-411">已停用 CoreWebEngine 或 W3SVC 伺服器功能</span><span class="sxs-lookup"><span data-stu-id="95788-411">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="95788-412">**作業系統例外狀況：** 必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="95788-412">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="95788-413">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-413">Troubleshooting:</span></span>

<span data-ttu-id="95788-414">確認已啟用適當的角色和功能。</span><span class="sxs-lookup"><span data-stu-id="95788-414">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="95788-415">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="95788-415">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="95788-416">不正確的網站實體路徑或遺失應用程式</span><span class="sxs-lookup"><span data-stu-id="95788-416">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="95788-417">**瀏覽器：** 403 禁止 - 存取被拒 **--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="95788-417">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="95788-418">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="95788-418">**Application Log:** No entry</span></span>

* <span data-ttu-id="95788-419">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-419">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="95788-420">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-420">Troubleshooting:</span></span>

<span data-ttu-id="95788-421">檢查 IIS 網站的 [基本設定]\*\*\*\* 和實體的應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="95788-421">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="95788-422">確認應用程式位於 IIS 網站**實體路徑**的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="95788-422">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="95788-423">角色不正確、ASP.NET Core 模組未安裝，或權限不正確</span><span class="sxs-lookup"><span data-stu-id="95788-423">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="95788-424">**瀏覽器：** 500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="95788-424">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="95788-425">**--或--** 無法顯示此頁面</span><span class="sxs-lookup"><span data-stu-id="95788-425">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="95788-426">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="95788-426">**Application Log:** No entry</span></span>

* <span data-ttu-id="95788-427">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-427">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="95788-428">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-428">Troubleshooting:</span></span>

* <span data-ttu-id="95788-429">確認已啟用適當的角色。</span><span class="sxs-lookup"><span data-stu-id="95788-429">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="95788-430">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="95788-430">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="95788-431">開啟 [程式和功能]\*\*\*\* 或 [應用程式與功能]\*\*\*\*，並確認已安裝 **Windows Server Hosting**。</span><span class="sxs-lookup"><span data-stu-id="95788-431">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="95788-432">如果已安裝的程式清單中沒有 **Windows Server Hosting**，請下載並安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="95788-432">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="95788-433">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="95788-433">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="95788-434">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="95788-434">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="95788-435">請確定**應用程式**集區 > **進程模型** > **Identity** 已設定為**ApplicationPoolIdentity** ，或自訂身分識別具有正確的許可權，可存取應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="95788-435">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="95788-436">若解除安裝 ASP.NET Core Hosting Bundle 並安裝舊版裝載套件組合，*applicationHost.config* 檔案不會包括 ASP.NET Core Module 的區段。</span><span class="sxs-lookup"><span data-stu-id="95788-436">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="95788-437">開啟位於 *%windir%/System32/inetsrv/config* 的 *applicationHost.config*，然後尋找 `<configuration><configSections><sectionGroup name="system.webServer">` 區段群組。</span><span class="sxs-lookup"><span data-stu-id="95788-437">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="95788-438">若區段群組中沒有 ASP.NET Core Module 的區段，請新增區段元素：</span><span class="sxs-lookup"><span data-stu-id="95788-438">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="95788-439">或者，您也可以安裝最新版的「ASP.NET Core 裝載套件組合」。</span><span class="sxs-lookup"><span data-stu-id="95788-439">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="95788-440">最新版本回溯相容，提供支援的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="95788-440">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="95788-441">不正確的 processPath、遺失 PATH 變數、未安裝裝載套件組合、未重新啟動系統/IIS、未安裝 VC++ 可轉散發套件或 dotnet.exe 存取違規</span><span class="sxs-lookup"><span data-stu-id="95788-441">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="95788-442">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="95788-442">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="95788-443">**應用程式記錄檔：** 實體根目錄為 ' C： PATH} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' \{ \' 無法使用命令列 ' "{...}" 來啟動進程</span><span class="sxs-lookup"><span data-stu-id="95788-443">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="95788-444">'，ErrorCode = ' 0x80070002：0。</span><span class="sxs-lookup"><span data-stu-id="95788-444">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="95788-445">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="95788-445">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

<span data-ttu-id="95788-446">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-446">Troubleshooting:</span></span>

* <span data-ttu-id="95788-447">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="95788-447">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="95788-448">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="95788-448">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="95788-449">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="95788-449">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="95788-450">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *processPath* 屬性，以確認它是 `dotnet` (適用於架構相依部署 (FDD)) 或 `.\{ASSEMBLY}.exe` (適用於[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd))。</span><span class="sxs-lookup"><span data-stu-id="95788-450">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="95788-451">若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="95788-451">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="95788-452">確認\*C:\Program Files\dotnet \\ \*存在於系統路徑設定中。</span><span class="sxs-lookup"><span data-stu-id="95788-452">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="95788-453">若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="95788-453">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="95788-454">確認應用程式集區使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。</span><span class="sxs-lookup"><span data-stu-id="95788-454">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="95788-455">確認 *C:\Program Files\dotnet* 和應用程式目錄上未針對應用程式集區使用者身分識別設定任何拒絕規則。</span><span class="sxs-lookup"><span data-stu-id="95788-455">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="95788-456">可能已部署 FDD 並安裝 .NET Core，但未重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="95788-456">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="95788-457">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="95788-457">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="95788-458">可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="95788-458">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="95788-459">如果尚未安裝 .NET Core 執行階段，請在系統上執行「.NET Core 裝載套件組合安裝程式」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="95788-459">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="95788-460">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="95788-460">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="95788-461">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="95788-461">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="95788-462">如果需要特定執行階段，請從 [.NET Download Archives](https://dotnet.microsoft.com/download/archives) (.NET 下載封存) 下載執行階段，並將它安裝在系統上。</span><span class="sxs-lookup"><span data-stu-id="95788-462">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="95788-463">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。</span><span class="sxs-lookup"><span data-stu-id="95788-463">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="95788-464">元素的引數不正確 \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="95788-464">Incorrect arguments of \<aspNetCore> element</span></span>

* <span data-ttu-id="95788-465">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="95788-465">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="95788-466">**應用程式記錄檔：** 實體根目錄為 ' C： PATH} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' \{ \' 無法使用命令列 ' "dotnet" 來啟動進程。 \{ASSEMBLY} .dll '，ErrorCode = ' 0x80004005：80008081。</span><span class="sxs-lookup"><span data-stu-id="95788-466">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="95788-467">**ASP.NET Core 模組 Stdout 記錄檔：** 要執行的應用程式不存在： ' PATH \{ ASSEMBLY} .dll '</span><span class="sxs-lookup"><span data-stu-id="95788-467">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

<span data-ttu-id="95788-468">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-468">Troubleshooting:</span></span>

* <span data-ttu-id="95788-469">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="95788-469">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="95788-470">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="95788-470">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="95788-471">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="95788-471">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="95788-472">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *arguments* 屬性，以確認它是 (a) `.\{ASSEMBLY}.dll` (適用於架構相依部署 (FDD))；或 (b) 不存在、空字串 (`arguments=""`)，或是應用程式的引數清單 (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`，適用於自封式部署 (SCD))。</span><span class="sxs-lookup"><span data-stu-id="95788-472">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

<span data-ttu-id="95788-473">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-473">Troubleshooting:</span></span>

<span data-ttu-id="95788-474">若為結構相依部署 (FDD)，請確認已在系統上安裝正確的執行階段。</span><span class="sxs-lookup"><span data-stu-id="95788-474">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="95788-475">已停止應用程式集區</span><span class="sxs-lookup"><span data-stu-id="95788-475">Stopped Application Pool</span></span>

* <span data-ttu-id="95788-476">**瀏覽器：** 503 服務無法使用</span><span class="sxs-lookup"><span data-stu-id="95788-476">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="95788-477">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="95788-477">**Application Log:** No entry</span></span>

* <span data-ttu-id="95788-478">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-478">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="95788-479">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-479">Troubleshooting:</span></span>

<span data-ttu-id="95788-480">確認「應用程式集區」不是處於「已停止」\*\* 狀態。</span><span class="sxs-lookup"><span data-stu-id="95788-480">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="95788-481">子應用程式包含 \<handlers> 區段</span><span class="sxs-lookup"><span data-stu-id="95788-481">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="95788-482">**瀏覽器：** HTTP 錯誤 500.19 - 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="95788-482">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="95788-483">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="95788-483">**Application Log:** No entry</span></span>

* <span data-ttu-id="95788-484">**ASP.NET Core 模組 Stdout 記錄檔：** 根應用程式的記錄檔隨即建立，並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="95788-484">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="95788-485">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-485">The sub-app's log file isn't created.</span></span>

<span data-ttu-id="95788-486">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-486">Troubleshooting:</span></span>

<span data-ttu-id="95788-487">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段。</span><span class="sxs-lookup"><span data-stu-id="95788-487">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="95788-488">stdout 記錄檔路徑不正確</span><span class="sxs-lookup"><span data-stu-id="95788-488">stdout log path incorrect</span></span>

* <span data-ttu-id="95788-489">**瀏覽器：** 應用程式正常回應。</span><span class="sxs-lookup"><span data-stu-id="95788-489">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="95788-490">**應用程式記錄檔：** 警告：無法建立 stdoutLogFile \\ ？ \{PATH} \ path_doesnt_exist \ stdout_ {處理序識別碼} _ {TIMESTAMP} .log，ErrorCode =-2147024893。</span><span class="sxs-lookup"><span data-stu-id="95788-490">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="95788-491">**ASP.NET Core 模組 Stdout 記錄檔：** 不會建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="95788-491">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="95788-492">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-492">Troubleshooting:</span></span>

* <span data-ttu-id="95788-493">*web.config* 中 `<aspNetCore>` 元素所指定的 `stdoutLogFile` 路徑不存在。</span><span class="sxs-lookup"><span data-stu-id="95788-493">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="95788-494">如需詳細資訊，請參閱[ASP.NET Core 模組：記錄建立和](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)重新導向。</span><span class="sxs-lookup"><span data-stu-id="95788-494">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="95788-495">應用程式集區使用者沒有 stdout 記錄檔路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="95788-495">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="95788-496">應用程式組態一般問題</span><span class="sxs-lookup"><span data-stu-id="95788-496">Application configuration general issue</span></span>

* <span data-ttu-id="95788-497">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="95788-497">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="95788-498">**應用程式記錄檔：** 實體根目錄為 ' C： path} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' 已 \{ \' 使用命令列 ' "C： \{ path} \{ ASSEMBLY} 建立進程。 {exe | dll} "'，但已當機或沒有回應，或未在指定的埠 ' {PORT} ' 上接聽，ErrorCode = ' {ERROR CODE} '</span><span class="sxs-lookup"><span data-stu-id="95788-498">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="95788-499">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="95788-499">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

<span data-ttu-id="95788-500">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="95788-500">Troubleshooting:</span></span>

<span data-ttu-id="95788-501">處理序無法啟動，最可能的原因是應用程式組態或程式設計問題。</span><span class="sxs-lookup"><span data-stu-id="95788-501">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="95788-502">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="95788-502">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>

::: moniker-end
