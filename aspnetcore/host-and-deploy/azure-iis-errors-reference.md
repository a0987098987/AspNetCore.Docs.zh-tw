---
title: "Azure 應用程式服務和 IIS 與 ASP.NET Core 的常見錯誤參考"
author: guardrex
description: "裝載 Azure 應用程式服務和 IIS 的 ASP.NET Core 應用程式時，請區分常見的錯誤。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 53b59045751153cd858e13769b5b42d5700e26d4
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Azure 應用程式服務和 IIS 與 ASP.NET Core 的常見錯誤參考

作者：[Luke Latham](https://github.com/guardrex)

以下的錯誤清單並非完整清單。 如果您遇到錯誤未列在此處，[開啟新議題](https://github.com/aspnet/Docs/issues/new)重現錯誤的詳細指示。

收集下列資訊：

* 瀏覽器的行為
* 應用程式事件記錄檔項目
* ASP.NET 核心模組 stdout 記錄項目

比較下列的一般錯誤的資訊。 如果找到相符項目，請遵循的疑難排解建議。

## <a name="installer-unable-to-obtain-vc-redistributable"></a>安裝程式無法取得 VC++ 可轉散發套件

* **安裝程式的例外狀況：**0x80072efd 或 0x80072f76 - 未指定的錯誤

* **安裝程式記錄例外狀況&#8224;：**錯誤 0x80072efd 或 0x80072f76：無法執行 EXE 套件

  &#8224;記錄位於 C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log。

疑難排解：

* 如果安裝伺服器裝載套件組合時，系統無法存取網際網路，導致安裝程式無法取得 *Microsoft Visual C++ 2015 可轉散發套件*，就會發生這個例外狀況。 取得從安裝[Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840)。 如果安裝程式失敗，伺服器可能不會接收裝載 framework 相依的部署 (與 FDD) 所需的.NET 核心執行階段。 如果裝載與 FDD，確認執行階段是否已安裝的程式中&amp;功能。 如有需要取得執行階段安裝從[.NET 下載](https://www.microsoft.com/net/download/core)。 安裝執行階段之後，從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動系統或重新啟動 IIS。

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>作業系統升級已移除 32 位元的 ASP.NET Core 模組

* **應用程式記錄檔：**無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。 資料即錯誤。

疑難排解：

* 作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。 如果之前已安裝的 ASP.NET 核心模組作業系統升級，然後任何應用程式集區時執行 32 位元模式在作業系統升級之後，就會發生此問題。 作業系統升級之後，請修復 ASP.NET Core 模組。 請參閱[安裝 .NET Core Windows Server 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)。 選取**修復**執行安裝程式時。

## <a name="platform-conflicts-with-rid"></a>平台發生 RID 衝突

* **瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：**應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\{路徑}\'無法啟動處理序的命令列使用 '"c:\\{PATH} {組件}。 {exe | dll}"'，錯誤碼 = ' 0x80004005: ff。

* **ASP.NET Core 模組記錄：**未處理例外狀況： System.BadImageFormatException： 無法載入檔案或組件 '{組件}.dll'。 嘗試載入了格式不正確的程式。

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。

* 確認`<PlatformTarget>`中*.csproj* RID 不相衝突。 例如，未指定`<PlatformTarget>`的`x86`和使用的 RID 發行`win10-x64`，藉由使用*dotnet 發行-c 版本-r win10 x64*或藉由設定`<RuntimeIdentifiers>`中*.csproj*至`win10-x64`。 專案發佈時沒有警告或錯誤，但因為上述在系統上記錄的例外狀況而失敗。

* 如果這個例外狀況時發生的 Azure 應用程式的部署升級應用程式和部署較新的組件，以手動方式刪除所有檔案從先前的部署。 部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI 端點錯誤或停止了網站

* **瀏覽器：**ERR_CONNECTION_REFUSED

* **應用程式記錄檔：**無項目

* **ASP.NET Core 模組記錄檔：**未建立記錄檔

疑難排解：

* 請確認正在使用正確的 URI 端點的應用程式。 請檢查繫結。

* 確認 IIS 網站不處於*已停止*狀態。

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>已停用 CoreWebEngine 或 W3SVC 伺服器功能

* **作業系統例外狀況：**必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。

疑難排解：

* 確認正確的角色和功能已啟用。 請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。

## <a name="incorrect-website-physical-path-or-app-missing"></a>不正確的網站實體路徑，或遺漏應用程式

* **瀏覽器：** 403 禁止 - 存取被拒**--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。

* **應用程式記錄檔：**無項目

* **ASP.NET Core 模組記錄檔：**未建立記錄檔

疑難排解：

* 請檢查 IIS 網站**基本設定**和實體的應用程式資料夾。 確認應用程式位於 IIS 網站上的資料夾**實體路徑**。

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>角色不正確、模組未安裝，或權限不正確。

* **瀏覽器：**500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。

* **應用程式記錄檔：**無項目

* **ASP.NET Core 模組記錄檔：**未建立記錄檔

疑難排解：

* 確認已啟用適當的角色。 請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。

* 請檢查 [程式和功能] 並確認 **Microsoft ASP.NET Core 模組** 已安裝。 如果已安裝的程式清單中沒有 **Microsoft ASP.NET Core 模組**，請安裝此模組。 請參閱[安裝 .NET Core Windows Server 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)。

* 請確定**應用程式集區** > **處理序模型** > **識別**設**ApplicationPoolIdentity**或自訂身分識別正確的權限來存取應用程式的部署資料夾。

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>不正確的 processPath, 遺失 PATH 變數, 未安裝裝載套件組合, 未重新啟動系統/IIS, 未安裝 VC++ 可轉散發套件, 或 dotnet.exe 存取違規

* **瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：**應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\\{PATH}\'無法啟動處理序的命令列使用 ' 」。\{組件}.exe"'，錯誤碼 = ' 0x80070002: 0。

* **ASP.NET Core 模組記錄檔：**已建立記錄檔，但卻是空的。

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。

* 請檢查*processPath*屬性`<aspNetCore>`中的項目*web.config*來確認它是*dotnet* framework 相依的部署 (與 FDD) 或*.\{組件}.exe*獨立部署 (SCD)。

* 若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。 確認系統 PATH 設定中有 *C:\Program Files\dotnet\*。

* 若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。 確認 AppPool 使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。 確認沒有拒絕規則上的 AppPool 使用者識別為設定*C:\Program Files\dotnet*和應用程式目錄。

* 與 FDD 部署和.NET Core 安裝不需要重新啟動 IIS。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。

* 與 FDD 可能部署而不需要在主機系統上安裝.NET 核心執行階段。 如果尚未安裝.NET 核心執行階段，執行**.NET 核心 Windows Server 裝載配套 installer**系統上。 請參閱[安裝 .NET Core Windows Server 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)。 如果嘗試在沒有網際網路連線的系統上安裝.NET 核心執行階段時，取得執行階段從[.NET 下載](https://www.microsoft.com/net/download/core)並執行裝載配套安裝程式安裝 ASP.NET 核心模組。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。

* 與 FDD 部署和*Microsoft Visual c + + 2015年可轉散發 (x64)*系統上未安裝。 取得從安裝[Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840)。

## <a name="incorrect-arguments-of-aspnetcore-element"></a>不正確的 \<aspNetCore\> 元素引數

* **瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：**應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\\{PATH}\'無法啟動處理序的命令列使用 '"dotnet"。\{組件}.dll '，錯誤碼 = ' 0x80004005: 80008081。

* **ASP.NET Core 模組記錄檔：**要執行的應用程式不存在: ' 路徑\{組件}.dll '

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。

* 檢查*引數*屬性`<aspNetCore>`中的項目*web.config*以確認它可能是 (a) *。\{組件}.dll* framework 相依的部署 (與 FDD); 或 (b) 不存在、 空字串 (*引數 =""*)，或應用程式的引數清單 (*引數 ="arg1，arg2，..."*)獨立部署 (SCD)。

## <a name="missing-net-framework-version"></a>遺失 .NET Framework 版本

* **瀏覽器：** 502.3 不正確的閘道 - 嘗試路由要求時發生連線錯誤。

* **應用程式記錄檔：** ErrorCode = 應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\\{PATH}\'無法啟動處理序的命令列使用 '"dotnet"。\{組件}.dll '，錯誤碼 = ' 0x80004005: 80008081。

* **ASP.NET Core 模組記錄檔：**遺失方法、檔案或組件例外狀況。 例外狀況中指定的方法、檔案或組件是 .NET Framework 方法、檔案或組件。

疑難排解：

* 安裝系統中遺失的 .NET Framework 版本。

* 針對架構相依性部署 (與 FDD)，請確認系統上安裝正確的執行階段。 如果從 1.1 升級專案至 2.0 時，部署到主機系統，以及造成此例外狀況，請確定 2.0 framework 是在主機系統上。

## <a name="stopped-application-pool"></a>已停止應用程式集區

* **瀏覽器：**503 服務無法使用

* **應用程式記錄檔：**無項目

* **ASP.NET Core 模組記錄檔：**未建立記錄檔

疑難排解

* 確認應用程式集區不在*已停止*狀態。

## <a name="iis-integration-middleware-not-implemented"></a>未實作 IIS Integration 中介軟體

* **瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：**應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\\{PATH}\'使用命令列建立程序 '"c:\\{PATH}\{組件}。 {exe | dll}"' 但可能當機或沒有不回應，或者未接聽指定連接埠 '{PORT}' ErrorCode = '0x800705b4'

* **ASP.NET Core 模組記錄檔：**記錄檔已建立並顯示一般作業。

疑難排解

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。

* 請確認：
  * IIS Integration 中介軟體為 referencedby 呼叫`UseIISIntegration`方法上的應用程式`WebHostBuilder`(ASP.NET Core 1.x)
  * 應用程式會使用`CreateDefaultBuilder`方法 (ASP.NET Core 2.x)。
  
  如需詳細資訊，請參閱[在 ASP.NET Core 中裝載](xref:fundamentals/hosting)。

## <a name="sub-application-includes-a-handlers-section"></a>子應用程式包含\<處理常式\>區段

* **瀏覽器：**HTTP 錯誤 500.19 - 內部伺服器錯誤

* **應用程式記錄檔：**無項目

* **ASP.NET Core 模組記錄：**記錄檔建立並顯示根應用程式的一般作業。 未建立子應用程式記錄檔。

疑難排解

* 請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段。

## <a name="application-configuration-general-issue"></a>應用程式組態一般問題

* **瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：**應用程式 'MACHINE/WEBROOT/APPHOST / {組件}' 與實體根' c:\\{PATH}\'使用命令列建立程序 '"c:\\{PATH}\{組件}。 {exe | dll}"' 但可能當機或沒有不回應，或者未接聽指定連接埠 '{PORT}' ErrorCode = '0x800705b4'

* **ASP.NET Core 模組記錄檔：**已建立記錄檔，但卻是空的。

疑難排解

* 這個一般例外狀況表示處理程序無法啟動，很可能是由於應用程式組態問題。 參考[目錄結構](xref:host-and-deploy/directory-structure)，確認應用程式的部署檔案，並會適當的資料夾和應用程式的組態檔是否都存在，以及包含應用程式和環境的正確設定。 如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。
