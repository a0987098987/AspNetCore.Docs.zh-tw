---
title: "Azure 應用程式服務和 IIS 與 ASP.NET Core 的常見錯誤參考"
author: guardrex
description: "裝載 Azure 應用程式服務和 IIS 的 ASP.NET Core 應用程式時，請區分常見的錯誤。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 58c5ec6bb2603499332698fd4225e2fe636256e4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="00da4-103">Azure 應用程式服務和 IIS 與 ASP.NET Core 的常見錯誤參考</span><span class="sxs-lookup"><span data-stu-id="00da4-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="00da4-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="00da4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="00da4-105">以下的錯誤清單並非完整清單。</span><span class="sxs-lookup"><span data-stu-id="00da4-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="00da4-106">如果您遇到錯誤未列在此處，[開啟新議題](https://github.com/aspnet/Docs/issues/new)重現錯誤的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="00da4-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="00da4-107">安裝程式無法取得 VC++ 可轉散發套件</span><span class="sxs-lookup"><span data-stu-id="00da4-107">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="00da4-108">**安裝程式的例外狀況：**0x80072efd 或 0x80072f76 - 未指定的錯誤</span><span class="sxs-lookup"><span data-stu-id="00da4-108">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="00da4-109">**安裝程式記錄例外狀況&#8224;：**錯誤 0x80072efd 或 0x80072f76：無法執行 EXE 套件</span><span class="sxs-lookup"><span data-stu-id="00da4-109">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="00da4-110">&#8224;記錄位於 C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log。</span><span class="sxs-lookup"><span data-stu-id="00da4-110">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="00da4-111">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="00da4-111">Troubleshooting:</span></span>

* <span data-ttu-id="00da4-112">如果安裝伺服器裝載套件組合時，系統無法存取網際網路，導致安裝程式無法取得 *Microsoft Visual C++ 2015 可轉散發套件*，就會發生這個例外狀況。</span><span class="sxs-lookup"><span data-stu-id="00da4-112">If the system doesn't have Internet access while installing the server hosting bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="00da4-113">取得從安裝[Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840)。</span><span class="sxs-lookup"><span data-stu-id="00da4-113">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="00da4-114">如果安裝程式失敗，伺服器可能不會接收裝載 framework 相依的部署 (與 FDD) 所需的.NET 核心執行階段。</span><span class="sxs-lookup"><span data-stu-id="00da4-114">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="00da4-115">如果裝載與 FDD，確認執行階段是否已安裝的程式中&amp;功能。</span><span class="sxs-lookup"><span data-stu-id="00da4-115">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="00da4-116">如有需要取得執行階段安裝從[.NET 下載](https://www.microsoft.com/net/download/core)。</span><span class="sxs-lookup"><span data-stu-id="00da4-116">If needed, obtain a runtime installer from [.NET Downloads](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="00da4-117">安裝執行階段之後，從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動系統或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="00da4-117">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="00da4-118">作業系統升級已移除 32 位元的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="00da4-118">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="00da4-119">**應用程式記錄檔：**無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。</span><span class="sxs-lookup"><span data-stu-id="00da4-119">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="00da4-120">資料即錯誤。</span><span class="sxs-lookup"><span data-stu-id="00da4-120">The data is the error.</span></span>

<span data-ttu-id="00da4-121">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="00da4-121">Troubleshooting:</span></span>

* <span data-ttu-id="00da4-122">作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。</span><span class="sxs-lookup"><span data-stu-id="00da4-122">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="00da4-123">如果之前已安裝的 ASP.NET 核心模組作業系統升級，然後任何應用程式集區時執行 32 位元模式在作業系統升級之後，就會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="00da4-123">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="00da4-124">作業系統升級之後，請修復 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="00da4-124">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="00da4-125">請參閱[安裝 .NET Core Windows Server 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="00da4-125">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="00da4-126">選取**修復**執行安裝程式時。</span><span class="sxs-lookup"><span data-stu-id="00da4-126">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="00da4-127">平台發生 RID 衝突</span><span class="sxs-lookup"><span data-stu-id="00da4-127">Platform conflicts with RID</span></span>

* <span data-ttu-id="00da4-128">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="00da4-128">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="00da4-129">**應用程式記錄檔：**應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\{路徑}\'無法啟動處理序的命令列使用 '"c:\\{PATH} {組件}。 {exe | dll}"'，錯誤碼 = ' 0x80004005: ff。</span><span class="sxs-lookup"><span data-stu-id="00da4-129">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="00da4-130">**ASP.NET Core 模組記錄：**未處理例外狀況： System.BadImageFormatException： 無法載入檔案或組件 '{組件}.dll'。</span><span class="sxs-lookup"><span data-stu-id="00da4-130">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="00da4-131">嘗試載入了格式不正確的程式。</span><span class="sxs-lookup"><span data-stu-id="00da4-131">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="00da4-132">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="00da4-132">Troubleshooting:</span></span>

* <span data-ttu-id="00da4-133">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="00da4-133">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="00da4-134">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="00da4-134">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="00da4-135">如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。</span><span class="sxs-lookup"><span data-stu-id="00da4-135">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="00da4-136">確認`<PlatformTarget>`中*.csproj* RID 不相衝突。</span><span class="sxs-lookup"><span data-stu-id="00da4-136">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="00da4-137">例如，未指定`<PlatformTarget>`的`x86`和使用的 RID 發行`win10-x64`，藉由使用*dotnet 發行-c 版本-r win10 x64*或藉由設定`<RuntimeIdentifiers>`中*.csproj*至`win10-x64`。</span><span class="sxs-lookup"><span data-stu-id="00da4-137">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="00da4-138">專案發佈時沒有警告或錯誤，但因為上述在系統上記錄的例外狀況而失敗。</span><span class="sxs-lookup"><span data-stu-id="00da4-138">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="00da4-139">如果這個例外狀況時發生的 Azure 應用程式的部署升級應用程式和部署較新的組件，以手動方式刪除所有檔案從先前的部署。</span><span class="sxs-lookup"><span data-stu-id="00da4-139">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="00da4-140">部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="00da4-140">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="00da4-141">URI 端點錯誤或停止了網站</span><span class="sxs-lookup"><span data-stu-id="00da4-141">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="00da4-142">**瀏覽器：**ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="00da4-142">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="00da4-143">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="00da4-143">**Application Log:** No entry</span></span>

* <span data-ttu-id="00da4-144">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="00da4-144">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="00da4-145">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="00da4-145">Troubleshooting:</span></span>

* <span data-ttu-id="00da4-146">請確認正在使用正確的 URI 端點的應用程式。</span><span class="sxs-lookup"><span data-stu-id="00da4-146">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="00da4-147">請檢查繫結。</span><span class="sxs-lookup"><span data-stu-id="00da4-147">Check the bindings.</span></span>

* <span data-ttu-id="00da4-148">確認 IIS 網站不處於*已停止*狀態。</span><span class="sxs-lookup"><span data-stu-id="00da4-148">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="00da4-149">已停用 CoreWebEngine 或 W3SVC 伺服器功能</span><span class="sxs-lookup"><span data-stu-id="00da4-149">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="00da4-150">**作業系統例外狀況：**必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="00da4-150">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="00da4-151">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="00da4-151">Troubleshooting:</span></span>

* <span data-ttu-id="00da4-152">確認正確的角色和功能已啟用。</span><span class="sxs-lookup"><span data-stu-id="00da4-152">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="00da4-153">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="00da4-153">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="00da4-154">不正確的網站實體路徑，或遺漏應用程式</span><span class="sxs-lookup"><span data-stu-id="00da4-154">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="00da4-155">**瀏覽器：** 403 禁止 - 存取被拒**--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="00da4-155">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="00da4-156">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="00da4-156">**Application Log:** No entry</span></span>

* <span data-ttu-id="00da4-157">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="00da4-157">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="00da4-158">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="00da4-158">Troubleshooting:</span></span>

* <span data-ttu-id="00da4-159">請檢查 IIS 網站**基本設定**和實體的應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="00da4-159">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="00da4-160">確認應用程式位於 IIS 網站上的資料夾**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="00da4-160">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="00da4-161">角色不正確、模組未安裝，或權限不正確。</span><span class="sxs-lookup"><span data-stu-id="00da4-161">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="00da4-162">**瀏覽器：**500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="00da4-162">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="00da4-163">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="00da4-163">**Application Log:** No entry</span></span>

* <span data-ttu-id="00da4-164">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="00da4-164">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="00da4-165">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="00da4-165">Troubleshooting:</span></span>

* <span data-ttu-id="00da4-166">確認已啟用適當的角色。</span><span class="sxs-lookup"><span data-stu-id="00da4-166">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="00da4-167">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="00da4-167">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="00da4-168">請檢查 [程式和功能] 並確認 **Microsoft ASP.NET Core 模組** 已安裝。</span><span class="sxs-lookup"><span data-stu-id="00da4-168">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="00da4-169">如果已安裝的程式清單中沒有 **Microsoft ASP.NET Core 模組**，請安裝此模組。</span><span class="sxs-lookup"><span data-stu-id="00da4-169">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="00da4-170">請參閱[安裝 .NET Core Windows Server 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="00da4-170">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span>

* <span data-ttu-id="00da4-171">請確定**應用程式集區** > **處理序模型** > **識別**設**ApplicationPoolIdentity**或自訂身分識別正確的權限來存取應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="00da4-171">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="00da4-172">不正確的 processPath, 遺失 PATH 變數, 未安裝裝載套件組合, 未重新啟動系統/IIS, 未安裝 VC++ 可轉散發套件, 或 dotnet.exe 存取違規</span><span class="sxs-lookup"><span data-stu-id="00da4-172">Incorrect processPath, missing PATH variable, hosting bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="00da4-173">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="00da4-173">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="00da4-174">**應用程式記錄檔：**應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\\{PATH}\'無法啟動處理序的命令列使用 ' 」。\{組件}.exe"'，錯誤碼 = ' 0x80070002: 0。</span><span class="sxs-lookup"><span data-stu-id="00da4-174">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="00da4-175">**ASP.NET Core 模組記錄檔：**已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="00da4-175">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="00da4-176">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="00da4-176">Troubleshooting:</span></span>

* <span data-ttu-id="00da4-177">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="00da4-177">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="00da4-178">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="00da4-178">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="00da4-179">如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。</span><span class="sxs-lookup"><span data-stu-id="00da4-179">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="00da4-180">請檢查*processPath*屬性`<aspNetCore>`中的項目*web.config*來確認它是*dotnet* framework 相依的部署 (與 FDD) 或*.\{組件}.exe*獨立部署 (SCD)。</span><span class="sxs-lookup"><span data-stu-id="00da4-180">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="00da4-181">若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="00da4-181">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="00da4-182">確認系統 PATH 設定中有 *C:\Program Files\dotnet\*。</span><span class="sxs-lookup"><span data-stu-id="00da4-182">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="00da4-183">若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="00da4-183">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="00da4-184">確認 AppPool 使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。</span><span class="sxs-lookup"><span data-stu-id="00da4-184">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="00da4-185">確認沒有拒絕規則上的 AppPool 使用者識別為設定*C:\Program Files\dotnet*和應用程式目錄。</span><span class="sxs-lookup"><span data-stu-id="00da4-185">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="00da4-186">與 FDD 部署和.NET Core 安裝不需要重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="00da4-186">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="00da4-187">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="00da4-187">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="00da4-188">與 FDD 可能部署而不需要在主機系統上安裝.NET 核心執行階段。</span><span class="sxs-lookup"><span data-stu-id="00da4-188">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="00da4-189">如果尚未安裝.NET 核心執行階段，執行**.NET 核心 Windows Server 裝載配套 installer**系統上。</span><span class="sxs-lookup"><span data-stu-id="00da4-189">If the .NET Core runtime hasn't been installed, run the **.NET Core Windows Server Hosting bundle installer** on the system.</span></span> <span data-ttu-id="00da4-190">請參閱[安裝 .NET Core Windows Server 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="00da4-190">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="00da4-191">如果嘗試在沒有網際網路連線的系統上安裝.NET 核心執行階段時，取得執行階段從[.NET 下載](https://www.microsoft.com/net/download/core)並執行裝載配套安裝程式安裝 ASP.NET 核心模組。</span><span class="sxs-lookup"><span data-stu-id="00da4-191">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET Downloads](https://www.microsoft.com/net/download/core) and run the hosting bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="00da4-192">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。</span><span class="sxs-lookup"><span data-stu-id="00da4-192">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="00da4-193">與 FDD 部署和*Microsoft Visual c + + 2015年可轉散發 (x64)*系統上未安裝。</span><span class="sxs-lookup"><span data-stu-id="00da4-193">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="00da4-194">取得從安裝[Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840)。</span><span class="sxs-lookup"><span data-stu-id="00da4-194">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="00da4-195">不正確的 \<aspNetCore\> 元素引數</span><span class="sxs-lookup"><span data-stu-id="00da4-195">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="00da4-196">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="00da4-196">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="00da4-197">**應用程式記錄檔：**應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\\{PATH}\'無法啟動處理序的命令列使用 '"dotnet"。\{組件}.dll '，錯誤碼 = ' 0x80004005: 80008081。</span><span class="sxs-lookup"><span data-stu-id="00da4-197">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="00da4-198">**ASP.NET Core 模組記錄檔：**要執行的應用程式不存在: ' 路徑\{組件}.dll '</span><span class="sxs-lookup"><span data-stu-id="00da4-198">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="00da4-199">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="00da4-199">Troubleshooting:</span></span>

* <span data-ttu-id="00da4-200">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="00da4-200">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="00da4-201">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="00da4-201">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="00da4-202">如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。</span><span class="sxs-lookup"><span data-stu-id="00da4-202">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="00da4-203">檢查*引數*屬性`<aspNetCore>`中的項目*web.config*以確認它可能是 (a) *。\{組件}.dll* framework 相依的部署 (與 FDD); 或 (b) 不存在、 空字串 (*引數 =""*)，或應用程式的引數清單 (*引數 ="arg1，arg2，..."*)獨立部署 (SCD)。</span><span class="sxs-lookup"><span data-stu-id="00da4-203">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="00da4-204">遺失 .NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="00da4-204">Missing .NET Framework version</span></span>

* <span data-ttu-id="00da4-205">**瀏覽器：** 502.3 不正確的閘道 - 嘗試路由要求時發生連線錯誤。</span><span class="sxs-lookup"><span data-stu-id="00da4-205">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="00da4-206">**應用程式記錄檔：** ErrorCode = 應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\\{PATH}\'無法啟動處理序的命令列使用 '"dotnet"。\{組件}.dll '，錯誤碼 = ' 0x80004005: 80008081。</span><span class="sxs-lookup"><span data-stu-id="00da4-206">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="00da4-207">**ASP.NET Core 模組記錄檔：**遺失方法、檔案或組件例外狀況。</span><span class="sxs-lookup"><span data-stu-id="00da4-207">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="00da4-208">例外狀況中指定的方法、檔案或組件是 .NET Framework 方法、檔案或組件。</span><span class="sxs-lookup"><span data-stu-id="00da4-208">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="00da4-209">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="00da4-209">Troubleshooting:</span></span>

* <span data-ttu-id="00da4-210">安裝系統中遺失的 .NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="00da4-210">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="00da4-211">針對架構相依性部署 (與 FDD)，請確認系統上安裝正確的執行階段。</span><span class="sxs-lookup"><span data-stu-id="00da4-211">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="00da4-212">如果從 1.1 升級專案至 2.0 時，部署到主機系統，以及造成此例外狀況，請確定 2.0 framework 是在主機系統上。</span><span class="sxs-lookup"><span data-stu-id="00da4-212">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="00da4-213">已停止應用程式集區</span><span class="sxs-lookup"><span data-stu-id="00da4-213">Stopped Application Pool</span></span>

* <span data-ttu-id="00da4-214">**瀏覽器：**503 服務無法使用</span><span class="sxs-lookup"><span data-stu-id="00da4-214">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="00da4-215">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="00da4-215">**Application Log:** No entry</span></span>

* <span data-ttu-id="00da4-216">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="00da4-216">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="00da4-217">疑難排解</span><span class="sxs-lookup"><span data-stu-id="00da4-217">Troubleshooting</span></span>

* <span data-ttu-id="00da4-218">確認應用程式集區不在*已停止*狀態。</span><span class="sxs-lookup"><span data-stu-id="00da4-218">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="00da4-219">未實作 IIS Integration 中介軟體</span><span class="sxs-lookup"><span data-stu-id="00da4-219">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="00da4-220">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="00da4-220">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="00da4-221">**應用程式記錄檔：**應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\\{PATH}\'使用命令列建立程序 '"c:\\{PATH}\{組件}。 {exe | dll}"' 但可能當機或沒有不回應，或者未接聽指定連接埠 '{PORT}' ErrorCode = '0x800705b4'</span><span class="sxs-lookup"><span data-stu-id="00da4-221">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="00da4-222">**ASP.NET Core 模組記錄檔：**記錄檔已建立並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="00da4-222">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="00da4-223">疑難排解</span><span class="sxs-lookup"><span data-stu-id="00da4-223">Troubleshooting</span></span>

* <span data-ttu-id="00da4-224">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="00da4-224">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="00da4-225">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="00da4-225">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="00da4-226">如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。</span><span class="sxs-lookup"><span data-stu-id="00da4-226">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="00da4-227">請確認：</span><span class="sxs-lookup"><span data-stu-id="00da4-227">Confirm that either:</span></span>
  * <span data-ttu-id="00da4-228">IIS Integration 中介軟體為 referencedby 呼叫`UseIISIntegration`方法上的應用程式`WebHostBuilder`(ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="00da4-228">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="00da4-229">應用程式會使用`CreateDefaultBuilder`方法 (ASP.NET Core 2.x)。</span><span class="sxs-lookup"><span data-stu-id="00da4-229">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="00da4-230">如需詳細資訊，請參閱[在 ASP.NET Core 中裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="00da4-230">See [Hosting in ASP.NET Core](xref:fundamentals/hosting) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="00da4-231">子應用程式包含\<處理常式\>區段</span><span class="sxs-lookup"><span data-stu-id="00da4-231">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="00da4-232">**瀏覽器：**HTTP 錯誤 500.19 - 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="00da4-232">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="00da4-233">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="00da4-233">**Application Log:** No entry</span></span>

* <span data-ttu-id="00da4-234">**ASP.NET Core 模組記錄：**記錄檔建立並顯示根應用程式的一般作業。</span><span class="sxs-lookup"><span data-stu-id="00da4-234">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="00da4-235">未建立子應用程式記錄檔。</span><span class="sxs-lookup"><span data-stu-id="00da4-235">Log file not created for the sub-app.</span></span>

<span data-ttu-id="00da4-236">疑難排解</span><span class="sxs-lookup"><span data-stu-id="00da4-236">Troubleshooting</span></span>

* <span data-ttu-id="00da4-237">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段。</span><span class="sxs-lookup"><span data-stu-id="00da4-237">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="00da4-238">應用程式組態一般問題</span><span class="sxs-lookup"><span data-stu-id="00da4-238">Application configuration general issue</span></span>

* <span data-ttu-id="00da4-239">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="00da4-239">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="00da4-240">**應用程式記錄檔：**應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\\{PATH}\'使用命令列建立程序 '"c:\\{PATH}\{組件}。 {exe | dll}"' 但可能當機或沒有不回應，或者未接聽指定連接埠 '{PORT}' ErrorCode = '0x800705b4'</span><span class="sxs-lookup"><span data-stu-id="00da4-240">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="00da4-241">**ASP.NET Core 模組記錄檔：**已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="00da4-241">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="00da4-242">疑難排解</span><span class="sxs-lookup"><span data-stu-id="00da4-242">Troubleshooting</span></span>

* <span data-ttu-id="00da4-243">這個一般例外狀況表示處理程序無法啟動，很可能是由於應用程式組態問題。</span><span class="sxs-lookup"><span data-stu-id="00da4-243">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="00da4-244">參考[目錄結構](xref:host-and-deploy/directory-structure)，確認應用程式的部署檔案，並會適當的資料夾和應用程式的組態檔是否都存在，以及包含應用程式和環境的正確設定。</span><span class="sxs-lookup"><span data-stu-id="00da4-244">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="00da4-245">如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。</span><span class="sxs-lookup"><span data-stu-id="00da4-245">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
