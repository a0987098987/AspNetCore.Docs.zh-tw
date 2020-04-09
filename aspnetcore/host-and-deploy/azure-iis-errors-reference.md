---
title: Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考
author: rick-anderson
description: 取得在 Azure Apps Service 與 IIS 上裝載 ASP.NET Core 應用程式時的常見錯誤疑難排解建議。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 635c4cf6f12e62ca7e795b3b3b47e9445b945551
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79511596"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考

::: moniker range=">= aspnetcore-2.2"

本主題介紹常見錯誤,並在 Azure 應用服務和 IIS 上託管 ASP.NET 核心應用時,針對特定錯誤提供故障排除建議。

有關常規故障排除指南,請參閱<xref:test/troubleshoot-azure-iis>。

收集下列資訊：

* 瀏覽器行為 (狀態碼和錯誤訊息)
* 應用程式事件記錄檔項目
  * Azure App Service &ndash; 請參閱<xref:test/troubleshoot-azure-iis>。
  * IIS
    1. 在 [Windows]**** 功能表中選取 [開始]****，鍵入「事件檢視器」**，然後按 **Enter** 鍵。
    1. 在 [事件檢視器]**** 開啟之後，展開提要欄位中的 [Windows 記錄檔]**[應用程式]** > ****。
* ASP.NET Core 模組 stdout 和偵錯記錄項目
  * Azure App Service &ndash; 請參閱<xref:test/troubleshoot-azure-iis>。
  * IIS &ndash; 遵循＜ASP.NET Core 模組＞主題中[記錄檔建立和重新導向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)和[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)小節的指示。

將錯誤資訊與下列常見錯誤進行比較。 如果找到相符情況，請依照疑難排解建議進行操作。

本主題中的錯誤清單並不完整。 如果您遇到這裡未列出的錯誤，請使用本主題底部的 [內容意見反應]**** 按鈕來開啟新的問題，並提供如何重現錯誤的詳細指示。

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>作業系統升級已移除 32 位元的 ASP.NET Core 模組

**應用程式記錄檔：** 無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。 資料即錯誤。

疑難排解：

作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。 如果先安裝 ASP.NET Core 模組再升級 OS，然後在 OS 升級之後以 32 位元模式執行任何應用程式集區，就會發生此問題。 作業系統升級之後，請修復 ASP.NET Core 模組。 請參考[安裝 .NET 核心託管套件](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。 執行安裝程式時，請選取 [修復]****。

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a>遺失網站延伸模組、已安裝 32 位元 (x86) 和 64 位元 (x64) 網站延伸模組，或錯誤的處理序位元集合

*適用於 Azure 應用程式服務所裝載的應用程式。*

* **瀏覽器:** HTTP 錯誤 500.0 - ANCM 行程內處理程式載入失敗

* **應用程式紀錄:** 調用 hostfxr 以查找進程內請求處理程序失敗,但未找到任何本機依賴項。 找不到同處理序要求處理常式。 從調用 hostfxr 捕獲的輸出:無法找到任何相容的框架版本。 找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。 無法啟動應用程式 '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'。

* **ASP.NET核心模組固定日誌:** 找不到任何相容的框架版本。 找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。

* **ASP.NET核心模組除錯紀錄:** 調用 hostfxr 以查找進程內請求處理程序失敗,但未找到任何本機依賴項。 這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。 失敗的 HRESULT 傳回: 0x8000ffff. 找不到同處理序要求處理常式。 無法找到任何相容的架構版本。 找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。

疑難排解：

* 如果在預覽執行階段中執行應用程式，請安裝與應用程式位元和應用程式執行階段版本相符的 32 位元 (x86) **或** 64 位元 (x64) 網站延伸模組。 **請勿同時安裝這兩個延伸模組或延伸模組的多個執行階段版本。**

  * ASP.NET Core {RUNTIME VERSION} (x86) 執行階段
  * ASP.NET Core {RUNTIME VERSION} (x64) 執行階段

  重新啟動應用程式。 請等候數秒，讓應用程式重新啟動。

* 如果在預覽執行階段中執行應用程式，而且已經安裝 32 位元 (x86) 和 64 位元 (x64) [網站延伸模組](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension)，請將與應用程式位元不符的網站延伸模組解除安裝。 移除網站延伸模組之後，請重新啟動應用程式。 請等候數秒，讓應用程式重新啟動。

* 如果在預覽執行階段中執行應用程式，且網站延伸模組位元與應用程式相符，請確認預覽網站延伸模組的「執行階段版本」** 是否與應用程式相符。

* 確認**應用程式設定**中應用程式的**平台**與應用程式位元相符。

如需詳細資訊，請參閱 <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>。

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a>已部署 x86 應用程式，但未啟用 32 位元應用程式的應用程式集區

* **瀏覽器:** HTTP 錯誤 500.30 - ANCM 行程內啟動失敗

* **應用程式紀錄:** 應用程式'/LM/W3SVC/5/ROOT'與物理根'[PATH]命中意外託管異常,異常代碼 = '0xe0434352'。 如需詳細資訊，請檢查 stderr 記錄檔。 實體根目錄為 '{PATH}' 的應用程式 '/LM/W3SVC/5/ROOT' 無法載入 CLR 和受控應用程式。 CLR 背景工作執行緒不當結束

* **ASP.NET核心模組固定日誌:** 日誌檔已創建但為空。

* **ASP.NET核心模組除錯紀錄:** 傳回的 HRESULT 失敗: 0x8007023e

SDK 會在發行獨立應用程式時攔截到此案例。 如果 RID 不符合平台目標 (例如 `win10-x64` RID 在專案檔中使用 `<PlatformTarget>x86</PlatformTarget>`)，SDK 就會產生錯誤。

疑難排解：

針對 x86 架構相依部署 (`<PlatformTarget>x86</PlatformTarget>`)，請啟用 32 位元應用程式的 IIS 應用程式集區。 在 [IIS 管理員] 中，開啟應用程式集區的 [進階設定]****，然後將 [啟用 32 位元應用程式]**** 設定為 [True]****。

## <a name="platform-conflicts-with-rid"></a>平台發生 RID 衝突

* **瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗

* **應用程式紀錄:** 應用程式「計算機/WEBROOT/APPHOST/[ASSEMBLY]」,物理根為"C:PATH]\{\'未能用命令行"C:PATH_ASSEMBLY}"\{啟動進程。[exe_dll]" ',錯誤代碼 = '0x80004005 : ff.

* **ASP.NET核心模組固定日誌:** 未處理異常:系統.BadImageFormat異常:無法載入檔或程式集"_ASSEMBLY_.dll"。 嘗試載入了格式不正確的程式。

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

* 如果在升級應用程式和部署更新的組件時，Azure 應用程式部署發生這個例外狀況，請手動刪除來自先前部署的所有檔案。 部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI 端點錯誤或停止了網站

* **瀏覽器:** ERR_CONNECTION_REFUSED **-- OR --** 無法連線

* **應用程式記錄檔：** 無項目

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

* **ASP.NET核心模組除錯紀錄:** 未建立紀錄檔。

疑難排解：

* 確認使用對應用程式正確的 URI 端點。 檢查繫結。

* 確認 IIS 網站不是處於「已停止」** 狀態。

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>已停用 CoreWebEngine 或 W3SVC 伺服器功能

**作業系統例外狀況：** 必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。

疑難排解：

確認已啟用適當的角色和功能。 請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。

## <a name="incorrect-website-physical-path-or-app-missing"></a>不正確的網站實體路徑或遺失應用程式

* **瀏覽器：** 403 禁止 - 存取被拒 **--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。

* **應用程式記錄檔：** 無項目

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

* **ASP.NET核心模組除錯紀錄:** 未建立紀錄檔。

疑難排解：

檢查 IIS 網站的 [基本設定]**** 和實體的應用程式資料夾。 確認應用程式位於 IIS 網站**實體路徑**的資料夾中。

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a>角色不正確、ASP.NET Core 模組未安裝，或權限不正確

* **瀏覽器：** 500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。 **--或--** 無法顯示此頁面

* **應用程式記錄檔：** 無項目

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

* **ASP.NET核心模組除錯紀錄:** 未建立紀錄檔。

疑難排解：

* 確認已啟用適當的角色。 請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。

* 開啟 [程式和功能]**** 或 [應用程式與功能]****，並確認已安裝 **Windows Server Hosting**。 如果已安裝的程式清單中沒有 **Windows Server Hosting**，請下載並安裝 .NET Core 裝載套件組合。

  [目前的 .NET Core 裝載套件組合安裝程式 (直接下載)](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。

* 確定 [應用程式集區]**[處理序模型]** > **[身分識別]** > **** 已設定為 **ApplicationPoolIdentity**，或自訂身分識別具有正確的權限，可存取應用程式的部署資料夾。

* 若解除安裝 ASP.NET Core Hosting Bundle 並安裝舊版裝載套件組合，*applicationHost.config* 檔案不會包括 ASP.NET Core Module 的區段。 開啟位於 *%windir%/System32/inetsrv/config* 的 *applicationHost.config*，然後尋找 `<configuration><configSections><sectionGroup name="system.webServer">` 區段群組。 若區段群組中沒有 ASP.NET Core Module 的區段，請新增區段元素：

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  或者，您也可以安裝最新版的「ASP.NET Core 裝載套件組合」。 最新版本回溯相容，提供支援的 ASP.NET Core 應用程式。

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>不正確的 processPath、遺失 PATH 變數、未安裝裝載套件組合、未重新啟動系統/IIS、未安裝 VC++ 可轉散發套件或 dotnet.exe 存取違規

* **瀏覽器:** HTTP 錯誤 500.0 - ANCM 行程內處理程式載入失敗

* **應用程式紀錄:** 應用程式"計算機/WEBROOT/APPHOST/[ASSEMBLY]",物理根為"C:PATH}\{\'未能啟動使用命令行"{...}"的進程。 ', 錯誤代碼 = '0x80070002 : 0. 應用程式 '{PATH}' 無法啟動。 在 '{PATH}' 找不到可執行檔。 無法啟動應用程式 '/LM/W3SVC/2/ROOT'，ErrorCode '0x8007023e'。

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

* **ASP.NET核心模組除錯紀錄:** 事件日誌:"應用程式'[PATH]'無法啟動。 在 '{PATH}' 找不到可執行檔。 傳回的 HRESULT 失敗: 0x8007023e

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

* 檢查 *web.config* 中 `<aspNetCore>` 元素上的 *processPath* 屬性，以確認它是 `dotnet` (適用於架構相依部署 (FDD)) 或 `.\{ASSEMBLY}.exe` (適用於[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd))。

* 若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。 確認系統 PATH 設定中存在*C:\程式檔案\\\點網*。

* 若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。 確認應用程式集區使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。 確認 *C:\Program Files\dotnet* 和應用程式目錄上未針對應用程式集區使用者身分識別設定任何拒絕規則。

* 可能已部署 FDD 並安裝 .NET Core，但未重新啟動 IIS。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。

* 可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。 如果尚未安裝 .NET Core 執行階段，請在系統上執行「.NET Core 裝載套件組合安裝程式」****。

  [目前的 .NET Core 裝載套件組合安裝程式 (直接下載)](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。

  如果需要特定執行階段，請從 [.NET Download Archives](https://dotnet.microsoft.com/download/archives) (.NET 下載封存) 下載執行階段，並將它安裝在系統上。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。

## <a name="incorrect-arguments-of-aspnetcore-element"></a>不正確的 \<aspNetCore> 元素引數

* **瀏覽器:** HTTP 錯誤 500.0 - ANCM 行程內處理程式載入失敗

* **應用程式紀錄:** 調用 hostfxr 以查找進程內請求處理程序失敗,但未找到任何本機依賴項。 這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。 找不到同處理序要求處理常式。 從呼叫 hostfxr 擷取的輸出:您是否打算執行 dotnet SDK 命令? 請安裝點網路 SDKhttps://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409從: 啟動應用程式失敗 '/LM/W3SVC/3/ROOT', 錯誤代碼 '0x8000ffff'.

* **ASP.NET核心模組固定日誌:** 您是否打算執行點網路 SDK 命令? 請從下列位置安裝 dotnet SDK：https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409

* **ASP.NET核心模組除錯紀錄:** 調用 hostfxr 以查找進程內請求處理程序失敗,但未找到任何本機依賴項。 這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。 返回的 HRESULT 失敗: 0x8000ffff 找不到行程內請求處理程式。 從呼叫 hostfxr 擷取的輸出:您是否打算執行 dotnet SDK 命令? 請安裝點網路 SDKhttps://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409從: 故障 HRESULT 傳回: 0x8000ffff

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

* 檢查 *web.config* 中 `<aspNetCore>` 元素上的 *arguments* 屬性，以確認它是 (a) `.\{ASSEMBLY}.dll` (適用於架構相依部署 (FDD))；或 (b) 不存在、空字串 (`arguments=""`)，或是應用程式的引數清單 (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`，適用於自封式部署 (SCD))。

## <a name="missing-net-core-shared-framework"></a>遺失 .NET Core 共用架構

* **瀏覽器:** HTTP 錯誤 500.0 - ANCM 行程內處理程式載入失敗

* **應用程式紀錄:** 調用 hostfxr 以查找進程內請求處理程序失敗,但未找到任何本機依賴項。 這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。 找不到同處理序要求處理常式。 從調用 hostfxr 捕獲的輸出:無法找到任何相容的框架版本。 找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}'。

無法啟動應用程式 '/LM/W3SVC/5/ROOT'，ErrorCode '0x8000ffff'。

* **ASP.NET核心模組固定日誌:** 找不到任何相容的框架版本。 找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}'。

* **ASP.NET核心模組除錯紀錄:** 傳回失敗 HRESULT: 0x8000ff

疑難排解：

若為結構相依部署 (FDD)，請確認已在系統上安裝正確的執行階段。

## <a name="stopped-application-pool"></a>已停止應用程式集區

* **瀏覽器：** 503 服務無法使用

* **應用程式記錄檔：** 無項目

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

* **ASP.NET核心模組除錯紀錄:** 未建立紀錄檔。

疑難排解：

確認「應用程式集區」不是處於「已停止」** 狀態。

## <a name="sub-application-includes-a-handlers-section"></a>子應用程式包含 \<handlers> 區段

* **瀏覽器：** HTTP 錯誤 500.19 - 內部伺服器錯誤

* **應用程式記錄檔：** 無項目

* **ASP.NET核心模組固定日誌:** 根應用的日誌檔已創建並顯示正常操作。 未建立子應用程式的記錄檔。

* **ASP.NET核心模組除錯紀錄:** 根應用的日誌檔已創建並顯示正常操作。 未建立子應用程式的記錄檔。

疑難排解：

請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段，或子應用程式未繼承父應用程式的處理常式。

父應用程式 *web.config* 的 `<system.webServer>` 區段位於 `<location>` 元素內。 屬性<xref:System.Configuration.SectionInformation.InheritInChildApplications*>設置為`false`指示[\<位置>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location)元素中指定的設置不會由駐留在父應用子目錄中的應用繼承。 如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。

## <a name="stdout-log-path-incorrect"></a>stdout 記錄檔路徑不正確

* **瀏覽器：** 應用程式正常回應。

* **應用程式紀錄:** 無法在 C:\程式檔\IIS_Asp.Net 核心模組\V2_aspnetcorev2.dll 中啟動固定重定向。 異常消息:HRESULT 0x80070005 返回於 {PATH}aspnetcoremodulev2_公共lib_檔輸出管理器.cpp:84。 無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中停止 stdout 重新導向。 異常消息:HRESULT 0x80070002 在 [PATH] 返回。 無法在 {PATH}\aspnetcorev2_inprocess.dll 中啟動 stdout 重新導向。

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

* **ASP.NET核心模組除錯紀錄:** 無法在 C:\程式檔\IIS_Asp.Net 核心模組\V2_aspnetcorev2.dll 中啟動固定重定向。 異常消息:HRESULT 0x80070005 返回於 {PATH}aspnetcoremodulev2_公共lib_檔輸出管理器.cpp:84。 無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中停止 stdout 重新導向。 異常消息:HRESULT 0x80070002 在 [PATH] 返回。 無法在 {PATH}\aspnetcorev2_inprocess.dll 中啟動 stdout 重新導向。

疑難排解：

* *web.config* 中 `<aspNetCore>` 元素所指定的 `stdoutLogFile` 路徑不存在。 有關詳細資訊,請參閱[ASP.NET 核心模組:日誌建立和重定向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)。

* 應用程式集區使用者沒有 stdout 記錄檔路徑的寫入權限。

## <a name="application-configuration-general-issue"></a>應用程式組態一般問題

* **瀏覽器:** HTTP 錯誤 500.0 - ANCM 行程內處理程式負載故障 **-- -- OR--** HTTP 錯誤 500.30 - ANCM 行程內啟動失敗

* **應用程式紀錄:** 變數

* **ASP.NET核心模組固定日誌:** 日誌檔被創建,但為空,或使用普通條目創建,直到應用的點失敗。

* **ASP.NET核心模組除錯紀錄:** 變數

疑難排解：

處理序無法啟動，最可能的原因是應用程式組態或程式設計問題。

如需詳細資訊，請參閱下列主題：

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

本主題介紹常見錯誤,並在 Azure 應用服務和 IIS 上託管 ASP.NET 核心應用時,針對特定錯誤提供故障排除建議。

有關常規故障排除指南,請參閱<xref:test/troubleshoot-azure-iis>。

收集下列資訊：

* 瀏覽器行為 (狀態碼和錯誤訊息)
* 應用程式事件記錄檔項目
  * Azure App Service &ndash; 請參閱<xref:test/troubleshoot-azure-iis>。
  * IIS
    1. 在 [Windows]**** 功能表中選取 [開始]****，鍵入「事件檢視器」**，然後按 **Enter** 鍵。
    1. 在 [事件檢視器]**** 開啟之後，展開提要欄位中的 [Windows 記錄檔]**[應用程式]** > ****。
* ASP.NET Core 模組 stdout 和偵錯記錄項目
  * Azure App Service &ndash; 請參閱<xref:test/troubleshoot-azure-iis>。
  * IIS &ndash; 遵循＜ASP.NET Core 模組＞主題中[記錄檔建立和重新導向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)和[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)小節的指示。

將錯誤資訊與下列常見錯誤進行比較。 如果找到相符情況，請依照疑難排解建議進行操作。

本主題中的錯誤清單並不完整。 如果您遇到這裡未列出的錯誤，請使用本主題底部的 [內容意見反應]**** 按鈕來開啟新的問題，並提供如何重現錯誤的詳細指示。

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>作業系統升級已移除 32 位元的 ASP.NET Core 模組

**應用程式記錄檔：** 無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。 資料即錯誤。

疑難排解：

作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。 如果先安裝 ASP.NET Core 模組再升級 OS，然後在 OS 升級之後以 32 位元模式執行任何應用程式集區，就會發生此問題。 作業系統升級之後，請修復 ASP.NET Core 模組。 請參考[安裝 .NET 核心託管套件](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。 執行安裝程式時，請選取 [修復]****。

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a>遺失網站延伸模組、已安裝 32 位元 (x86) 和 64 位元 (x64) 網站延伸模組，或錯誤的處理序位元集合

*適用於 Azure 應用程式服務所裝載的應用程式。*

* **瀏覽器:** HTTP 錯誤 500.0 - ANCM 行程內處理程式載入失敗

* **應用程式紀錄:** 調用 hostfxr 以查找進程內請求處理程序失敗,但未找到任何本機依賴項。 找不到同處理序要求處理常式。 從調用 hostfxr 捕獲的輸出:無法找到任何相容的框架版本。 找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。 無法啟動應用程式 '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'。

* **ASP.NET核心模組固定日誌:** 找不到任何相容的框架版本。 找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。

疑難排解：

* 如果在預覽執行階段中執行應用程式，請安裝與應用程式位元和應用程式執行階段版本相符的 32 位元 (x86) **或** 64 位元 (x64) 網站延伸模組。 **請勿同時安裝這兩個延伸模組或延伸模組的多個執行階段版本。**

  * ASP.NET Core {RUNTIME VERSION} (x86) 執行階段
  * ASP.NET Core {RUNTIME VERSION} (x64) 執行階段

  重新啟動應用程式。 請等候數秒，讓應用程式重新啟動。

* 如果在預覽執行階段中執行應用程式，而且已經安裝 32 位元 (x86) 和 64 位元 (x64) [網站延伸模組](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension)，請將與應用程式位元不符的網站延伸模組解除安裝。 移除網站延伸模組之後，請重新啟動應用程式。 請等候數秒，讓應用程式重新啟動。

* 如果在預覽執行階段中執行應用程式，且網站延伸模組位元與應用程式相符，請確認預覽網站延伸模組的「執行階段版本」** 是否與應用程式相符。

* 確認**應用程式設定**中應用程式的**平台**與應用程式位元相符。

如需詳細資訊，請參閱 <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>。

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a>已部署 x86 應用程式，但未啟用 32 位元應用程式的應用程式集區

* **瀏覽器:** HTTP 錯誤 500.30 - ANCM 行程內啟動失敗

* **應用程式紀錄:** 應用程式'/LM/W3SVC/5/ROOT'與物理根'[PATH]命中意外託管異常,異常代碼 = '0xe0434352'。 如需詳細資訊，請檢查 stderr 記錄檔。 實體根目錄為 '{PATH}' 的應用程式 '/LM/W3SVC/5/ROOT' 無法載入 CLR 和受控應用程式。 CLR 背景工作執行緒不當結束

* **ASP.NET核心模組固定日誌:** 日誌檔已創建但為空。

SDK 會在發行獨立應用程式時攔截到此案例。 如果 RID 不符合平台目標 (例如 `win10-x64` RID 在專案檔中使用 `<PlatformTarget>x86</PlatformTarget>`)，SDK 就會產生錯誤。

疑難排解：

針對 x86 架構相依部署 (`<PlatformTarget>x86</PlatformTarget>`)，請啟用 32 位元應用程式的 IIS 應用程式集區。 在 [IIS 管理員] 中，開啟應用程式集區的 [進階設定]****，然後將 [啟用 32 位元應用程式]**** 設定為 [True]****。

## <a name="platform-conflicts-with-rid"></a>平台發生 RID 衝突

* **瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗

* **應用程式紀錄:** 應用程式「計算機/WEBROOT/APPHOST/[ASSEMBLY]」,物理根為"C:PATH]\{\'未能用命令行"C:PATH_ASSEMBLY}"\{啟動進程。[exe_dll]" ',錯誤代碼 = '0x80004005 : ff.

* **ASP.NET核心模組固定日誌:** 未處理異常:系統.BadImageFormat異常:無法載入檔或程式集"_ASSEMBLY_.dll"。 嘗試載入了格式不正確的程式。

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

* 如果在升級應用程式和部署更新的組件時，Azure 應用程式部署發生這個例外狀況，請手動刪除來自先前部署的所有檔案。 部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI 端點錯誤或停止了網站

* **瀏覽器:** ERR_CONNECTION_REFUSED **-- OR --** 無法連線

* **應用程式記錄檔：** 無項目

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

疑難排解：

* 確認使用對應用程式正確的 URI 端點。 檢查繫結。

* 確認 IIS 網站不是處於「已停止」** 狀態。

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>已停用 CoreWebEngine 或 W3SVC 伺服器功能

**作業系統例外狀況：** 必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。

疑難排解：

確認已啟用適當的角色和功能。 請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。

## <a name="incorrect-website-physical-path-or-app-missing"></a>不正確的網站實體路徑或遺失應用程式

* **瀏覽器：** 403 禁止 - 存取被拒 **--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。

* **應用程式記錄檔：** 無項目

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

疑難排解：

檢查 IIS 網站的 [基本設定]**** 和實體的應用程式資料夾。 確認應用程式位於 IIS 網站**實體路徑**的資料夾中。

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a>角色不正確、ASP.NET Core 模組未安裝，或權限不正確

* **瀏覽器：** 500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。 **--或--** 無法顯示此頁面

* **應用程式記錄檔：** 無項目

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

疑難排解：

* 確認已啟用適當的角色。 請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。

* 開啟 [程式和功能]**** 或 [應用程式與功能]****，並確認已安裝 **Windows Server Hosting**。 如果已安裝的程式清單中沒有 **Windows Server Hosting**，請下載並安裝 .NET Core 裝載套件組合。

  [目前的 .NET Core 裝載套件組合安裝程式 (直接下載)](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。

* 確定 [應用程式集區]**[處理序模型]** > **[身分識別]** > **** 已設定為 **ApplicationPoolIdentity**，或自訂身分識別具有正確的權限，可存取應用程式的部署資料夾。

* 若解除安裝 ASP.NET Core Hosting Bundle 並安裝舊版裝載套件組合，*applicationHost.config* 檔案不會包括 ASP.NET Core Module 的區段。 開啟位於 *%windir%/System32/inetsrv/config* 的 *applicationHost.config*，然後尋找 `<configuration><configSections><sectionGroup name="system.webServer">` 區段群組。 若區段群組中沒有 ASP.NET Core Module 的區段，請新增區段元素：

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  或者，您也可以安裝最新版的「ASP.NET Core 裝載套件組合」。 最新版本回溯相容，提供支援的 ASP.NET Core 應用程式。

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>不正確的 processPath、遺失 PATH 變數、未安裝裝載套件組合、未重新啟動系統/IIS、未安裝 VC++ 可轉散發套件或 dotnet.exe 存取違規

* **瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗

* **應用程式紀錄:** 應用程式"計算機/WEBROOT/APPHOST/[ASSEMBLY]",物理根為"C:PATH}\{\'未能啟動使用命令行"{...}"的進程。 ', 錯誤代碼 = '0x80070002 : 0.

* **ASP.NET核心模組固定日誌:** 日誌檔已創建但為空。

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

* 檢查 *web.config* 中 `<aspNetCore>` 元素上的 *processPath* 屬性，以確認它是 `dotnet` (適用於架構相依部署 (FDD)) 或 `.\{ASSEMBLY}.exe` (適用於[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd))。

* 若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。 確認系統 PATH 設定中存在*C:\程式檔案\\\點網*。

* 若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。 確認應用程式集區使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。 確認 *C:\Program Files\dotnet* 和應用程式目錄上未針對應用程式集區使用者身分識別設定任何拒絕規則。

* 可能已部署 FDD 並安裝 .NET Core，但未重新啟動 IIS。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。

* 可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。 如果尚未安裝 .NET Core 執行階段，請在系統上執行「.NET Core 裝載套件組合安裝程式」****。

  [目前的 .NET Core 裝載套件組合安裝程式 (直接下載)](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。

  如果需要特定執行階段，請從 [.NET Download Archives](https://dotnet.microsoft.com/download/archives) (.NET 下載封存) 下載執行階段，並將它安裝在系統上。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。

## <a name="incorrect-arguments-of-aspnetcore-element"></a>不正確的 \<aspNetCore> 元素引數

* **瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗

* **應用程式紀錄:** 應用程式「計算機/WEBROOT/APPHOST/[ASSEMBLY]」,物理根為"C:PATH}"\{\'未能啟動命令行"點網"的進程。\{ASSEMBLY_.dll',錯誤代碼 = '0x80004005 : 80008081。

* **ASP.NET核心模組固定日誌:** 要執行的應用程式不存在:"PATH\{ASSEMBLY_.dll"

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

* 檢查 *web.config* 中 `<aspNetCore>` 元素上的 *arguments* 屬性，以確認它是 (a) `.\{ASSEMBLY}.dll` (適用於架構相依部署 (FDD))；或 (b) 不存在、空字串 (`arguments=""`)，或是應用程式的引數清單 (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`，適用於自封式部署 (SCD))。

疑難排解：

若為結構相依部署 (FDD)，請確認已在系統上安裝正確的執行階段。

## <a name="stopped-application-pool"></a>已停止應用程式集區

* **瀏覽器：** 503 服務無法使用

* **應用程式記錄檔：** 無項目

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

疑難排解：

確認「應用程式集區」不是處於「已停止」** 狀態。

## <a name="sub-application-includes-a-handlers-section"></a>子應用程式包含 \<handlers> 區段

* **瀏覽器：** HTTP 錯誤 500.19 - 內部伺服器錯誤

* **應用程式記錄檔：** 無項目

* **ASP.NET核心模組固定日誌:** 根應用的日誌檔已創建並顯示正常操作。 未建立子應用程式的記錄檔。

疑難排解：

請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段。

## <a name="stdout-log-path-incorrect"></a>stdout 記錄檔路徑不正確

* **瀏覽器：** 應用程式正常回應。

* **應用程式紀錄:** 警告: 無法建立停滯紀錄檔\\?\{PATH_path_doesnt_exist[stdout_[進程 ID][TIMESTAMP].log,錯誤代碼 = -2147024893。

* **ASP.NET核心模組固定日誌:** 未建立紀錄檔。

疑難排解：

* *web.config* 中 `<aspNetCore>` 元素所指定的 `stdoutLogFile` 路徑不存在。 有關詳細資訊,請參閱[ASP.NET 核心模組:日誌建立和重定向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)。

* 應用程式集區使用者沒有 stdout 記錄檔路徑的寫入權限。

## <a name="application-configuration-general-issue"></a>應用程式組態一般問題

* **瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗

* **應用程式紀錄:** 應用程式「\{電腦/WEBROOT/APPHOST/[ASSEMBLY]」,物理根為"C:PATH]\'創建過程,命令行「C:PATH_ASSEMBLY}」。。\{ \{{exe_dll}" ' 但崩潰或未回應或未偵聽給定埠 "[PORT]",錯誤代碼 = "[錯誤代碼]"

* **ASP.NET核心模組固定日誌:** 日誌檔已創建但為空。

疑難排解：

處理序無法啟動，最可能的原因是應用程式組態或程式設計問題。

如需詳細資訊，請參閱下列主題：

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>

::: moniker-end
