---
title: Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考
author: guardrex
description: 區分在 Azure Apps Service 與 IIS 上裝載 ASP.NET Core 應用程式時的常見錯誤。
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 30b7f1d8e1cfdfd3d1db865ff428eb2094a84d32
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277315"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考

作者：[Luke Latham](https://github.com/guardrex)

以下的錯誤清單並非完整清單。 如果您遇到這裡未列出的錯誤，請[提出新問題](https://github.com/aspnet/Docs/issues/new)並提供可重現錯誤的詳細指示。

收集下列資訊：

* 瀏覽器行為
* 應用程式事件記錄檔項目
* ASP.NET Core 模組 stdout 記錄檔項目

將資訊與下列常見錯誤進行比較。 如果找到相符情況，請依照疑難排解建議進行操作。

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>安裝程式無法取得 VC++ 可轉散發套件

* **安裝程式的例外狀況：** 0x80072efd 或 0x80072f76 - 未指定的錯誤

* **安裝程式記錄例外狀況&#8224;：** 錯誤 0x80072efd 或 0x80072f76：無法執行 EXE 套件

  &#8224;記錄位於 C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log。

疑難排解：

* 安裝「裝載套件組合」時，如果系統無法存取網際網路，以致安裝程式無法取得 *Microsoft Visual C++ 2015 可轉散發套件*，就會發生這個例外狀況。 請從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53840)取得安裝程式。 如果安裝程式失敗，伺服器可能就無法收到裝載架構相依部署 (FDD) 所需的 .NET Core 執行階段。 如果要裝載 FDD，請確認已在 [程式和功能] 中安裝執行階段。 如有需要，請從 [.NET 下載](https://www.microsoft.com/net/download/all)取得執行階段安裝程式。 安裝執行階段之後，從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動系統或重新啟動 IIS。

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>作業系統升級已移除 32 位元的 ASP.NET Core 模組

* **應用程式記錄檔：** 無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。 資料即錯誤。

疑難排解：

* 作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。 如果先安裝 ASP.NET Core 模組再升級作業系統，然後在作業系統升級之後以 32 位元模式執行任何 AppPool，就會發生此問題。 作業系統升級之後，請修復 ASP.NET Core 模組。 請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。 執行安裝程式時，請選取 [修復]。

## <a name="platform-conflicts-with-rid"></a>平台發生 RID 衝突

* **瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：** 實體根目錄為 'C:\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 無法使用命令列 '"C:\\{PATH}{assembly}.{exe|dll}" ' 來啟動處理序，ErrorCode = '0x80004005 : ff。

* **ASP.NET Core 模組記錄檔：** 未處理的例外狀況：System.BadImageFormatException：無法載入檔案或組件 '{assembly}.dll'。 嘗試載入了格式不正確的程式。

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。

* 確認 *.csproj* 中的 `<PlatformTarget>` 未與 RID 衝突。 例如，藉由使用 *dotnet publish -c Release -r win10-x64*，或將 *.csproj* 中的 `<RuntimeIdentifiers>` 設定為 `win10-x64`，不指定 `x86` 的 `<PlatformTarget>` 並以 `win10-x64` 的 RID 發佈。 專案發佈時沒有警告或錯誤，但因為上述在系統上記錄的例外狀況而失敗。

* 如果在升級應用程式和部署更新的組件時，Azure 應用程式部署發生這個例外狀況，請手動刪除來自先前部署的所有檔案。 部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI 端點錯誤或停止了網站

* **瀏覽器：** ERR_CONNECTION_REFUSED

* **應用程式記錄檔：** 無項目

* **ASP.NET Core 模組記錄檔：** 未建立記錄檔

疑難排解：

* 確認使用正確的應用程式 URI 端點。 檢查繫結。

* 確認 IIS 網站不是處於「已停止」狀態。

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>已停用 CoreWebEngine 或 W3SVC 伺服器功能

* **作業系統例外狀況：** 必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。

疑難排解：

* 確認已啟用適當的角色和功能。 請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。

## <a name="incorrect-website-physical-path-or-app-missing"></a>不正確的網站實體路徑或遺失應用程式

* **瀏覽器：** 403 禁止 - 存取被拒 **--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。

* **應用程式記錄檔：** 無項目

* **ASP.NET Core 模組記錄檔：** 未建立記錄檔

疑難排解：

* 檢查 IIS 網站的 [基本設定] 和實體的應用程式資料夾。 確認應用程式位於 IIS 網站**實體路徑**的資料夾中。

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>角色不正確、模組未安裝，或權限不正確。

* **瀏覽器：** 500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。

* **應用程式記錄檔：** 無項目

* **ASP.NET Core 模組記錄檔：** 未建立記錄檔

疑難排解：

* 確認已啟用適當的角色。 請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。

* 請檢查 [程式和功能] 並確認 **Microsoft ASP.NET Core 模組** 已安裝。 如果已安裝的程式清單中沒有 **Microsoft ASP.NET Core 模組**，請安裝此模組。 請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。

* 確定 [應用程式集區] > [處理序模型] > [身分識別] 已設定為 **ApplicationPoolIdentity**，或自訂身分識別具有正確的權限，可存取應用程式的部署資料夾。

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>不正確的 processPath、遺失 PATH 變數、未安裝裝載套件組合、未重新啟動系統/IIS、未安裝 VC++ 可轉散發套件或 dotnet.exe 存取違規

* **瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：** 實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 無法使用命令列 '".\{assembly}.exe" ' 來啟動處理序，ErrorCode = '0x80070002 : 0。

* **ASP.NET Core 模組記錄檔：** 已建立記錄檔，但卻是空的。

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。

* 檢查 *web.config* 中 `<aspNetCore>` 元素上的 *processPath* 屬性，以確認它是 *dotnet* (適用於結構相依部署 (FDD)) 或 *.\{assembly}.exe* (適用於自封式部署 (SCD))。

* 若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。 確認系統 PATH 設定中有 *C:\Program Files\dotnet\*。

* 若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。 確認 AppPool 使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。 確認 *C:\Program Files\dotnet* 和應用程式目錄上未針對 AppPool 使用者身分識別設定任何拒絕規則。

* 可能已部署 FDD 並安裝 .NET Core，但未重新啟動 IIS。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。

* 可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。 如果尚未安裝 .NET Core 執行階段，請在系統上執行「.NET Core 裝載套件組合安裝程式」。 請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。 如果要嘗試在沒有網際網路連線的系統上安裝 .NET Core 執行階段，請從 [.NET 所有下載](https://www.microsoft.com/net/download/all)取得執行階段，然後執行「裝載套件組合」安裝程式來安裝 ASP.NET Core 模組。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。

* 可能已部署 FDD，但系統上未安裝 *Microsoft Visual C++ 2015 可轉散發套件 (x64)*。 請從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53840)取得安裝程式。

## <a name="incorrect-arguments-of-aspnetcore-element"></a>不正確的 \<aspNetCore\> 元素引數

* **瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：** 實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 無法使用命令列 '"dotnet" .\{assembly}.dll' 來啟動處理序，ErrorCode = '0x80004005 : 80008081。

* **ASP.NET Core 模組記錄檔：** 要執行的應用程式不存在：'PATH\{assembly}.dll'

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。

* 檢查 *web.config* 中 `<aspNetCore>` 元素上的 *arguments* 屬性，以確認它是 (a) *.\{assembly}.dll* (適用於結構相依部署 (FDD))；或 (b) 不存在、空字串 (*arguments=""*)，或是應用程式的引數清單 (*arguments="arg1, arg2, ..."*；適用於自封式部署 (SCD))。

## <a name="missing-net-framework-version"></a>遺失 .NET Framework 版本

* **瀏覽器：** 502.3 不正確的閘道 - 嘗試路由要求時發生連線錯誤。

* **應用程式記錄檔：** ErrorCode = 實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 無法使用命令列 '"dotnet" .\{assembly}.dll' 來啟動處理序，ErrorCode = '0x80004005 : 80008081。

* **ASP.NET Core 模組記錄檔：** 遺失方法、檔案或組件例外狀況。 例外狀況中指定的方法、檔案或組件是 .NET Framework 方法、檔案或組件。

疑難排解：

* 安裝系統中遺失的 .NET Framework 版本。

* 若為結構相依部署 (FDD)，請確認已在系統上安裝正確的執行階段。 如果將專案從 1.1 升級到 2.0 再部署到主控系統，然後發生這個例外狀況，請確定主控系統上有 2.0 架構。

## <a name="stopped-application-pool"></a>已停止應用程式集區

* **瀏覽器：** 503 服務無法使用

* **應用程式記錄檔：** 無項目

* **ASP.NET Core 模組記錄檔：** 未建立記錄檔

疑難排解

* 確認「應用程式集區」不是處於「已停止」狀態。

## <a name="iis-integration-middleware-not-implemented"></a>未實作 IIS Integration 中介軟體

* **瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：** 實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 已使用命令列 '"C:\\{PATH}\{assembly}.{exe|dll}" ' 來建立處理序，但可能當機、沒有回應或未在指定的連接埠 '{PORT}' 進行接聽，ErrorCode = '0x800705b4'

* **ASP.NET Core 模組記錄檔：** 記錄檔已建立並顯示一般作業。

疑難排解

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。

* 確認下列其中一項：
  * 參考 IIS 整合中介軟體時，是藉由呼叫應用程式 `WebHostBuilder` 上的 `UseIISIntegration` 來參考 (ASP.NET Core 1.x)
  * 應用程式使用 `CreateDefaultBuilder` 方法 (ASP.NET Core 2.x).
  
  如需詳細資料，請參閱 [ASP.NET Core 中的主機](xref:fundamentals/host/index)。

## <a name="sub-application-includes-a-handlers-section"></a>子應用程式包含\<處理常式\>區段

* **瀏覽器：** HTTP 錯誤 500.19 - 內部伺服器錯誤

* **應用程式記錄檔：** 無項目

* **ASP.NET Core 模組記錄檔：** 記錄檔已建立並顯示根應用程式的一般作業。 未建立子應用程式的記錄檔。

疑難排解

* 請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段。

## <a name="stdout-log-path-incorrect"></a>stdout 記錄檔路徑不正確

* **瀏覽器：** 應用程式正常回應。

* **應用程式記錄檔：** 警告：無法建立 stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log，ErrorCode = -2147024893。

* **ASP.NET Core 模組記錄檔：** 未建立記錄檔

疑難排解

* *web.config* 中 `<aspNetCore>` 元素所指定的 `stdoutLogFile` 路徑不存在。 如需詳細資訊，請參閱 ASP.NET Core 模組設定參考主題的[記錄檔建立和重新導向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)。

## <a name="application-configuration-general-issue"></a>應用程式組態一般問題

* **瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：** 實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' 已使用命令列 '"C:\\{PATH}\{assembly}.{exe|dll}" ' 來建立處理序，但可能當機、沒有回應或未在指定的連接埠 '{PORT}' 進行接聽，ErrorCode = '0x800705b4'

* **ASP.NET Core 模組記錄檔：** 已建立記錄檔，但卻是空的。

疑難排解

* 這個一般例外狀況指出處理序無法啟動，最可能的原因是應用程式設定問題。 參考[目錄結構](xref:host-and-deploy/directory-structure)，確認應用程式的已部署檔案和資料夾都適當，且應用程式的設定檔存在並包含應用程式和環境的正確設定。 如需詳細資訊，請參閱[疑難排解](xref:host-and-deploy/iis/troubleshoot)。
