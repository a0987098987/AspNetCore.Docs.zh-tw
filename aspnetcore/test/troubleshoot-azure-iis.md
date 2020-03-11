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
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a>疑難排解 Azure App Service 和 IIS 上的 ASP.NET Core

作者：[Justin Kotalik](https://github.com/jkotalik)

::: moniker range=">= aspnetcore-3.0"

本文提供有關一般應用程式啟動錯誤的資訊，以及如何在將應用程式部署至 Azure App Service 或 IIS 時診斷錯誤的指示：

[應用程式啟動錯誤](#app-startup-errors)  
說明常見的啟動 HTTP 狀態碼案例。

[針對 Azure App Service 進行疑難排解](#troubleshoot-on-azure-app-service)  
提供部署至 Azure App Service 之應用程式的疑難排解建議。

[針對 IIS 進行疑難排解](#troubleshoot-on-iis)  
提供部署至 IIS 或在本機 IIS Express 上執行之應用程式的疑難排解建議。 本指南適用于 Windows Server 和 Windows 桌面部署。

[清除套件快取](#clear-package-caches)  
說明當一致封裝在執行主要升級或變更封裝版本時，中斷應用程式時該怎麼辦。

[其他資源](#additional-resources)  
列出其他疑難排解主題。

## <a name="app-startup-errors"></a>應用程式啟動錯誤

在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。 使用本主題中的建議，可以診斷在本機進行偵錯工具時所發生的*502.5-進程失敗*或*500.30 啟動失敗*。

### <a name="40314-forbidden"></a>403.14 禁止

應用程式無法啟動。 會記錄下列錯誤：

```
The Web server is configured to not list the contents of this directory.
```

此錯誤通常是因為主控系統上的部署中斷所造成，其中包括下列任何一種情況：

* 應用程式會部署到裝載系統上錯誤的資料夾。
* 部署進程無法將應用程式的所有檔案和資料夾移到主控系統上的部署資料夾。
* 部署中遺漏*web.config*檔案，或*web.config*檔案內容的格式不正確。

執行下列步驟：

1. 刪除主控系統上部署資料夾中的所有檔案和資料夾。
1. 使用一般部署方法（例如 Visual Studio、PowerShell 或手動部署），將應用程式的 [*發佈*] 資料夾的內容重新部署至主機系統：
   * 請確認*web.config*檔案存在於部署中，而且其內容正確。
   * 在 Azure App Service 上裝載時，請確認應用程式已部署至 `D:\home\site\wwwroot` 資料夾。
   * 當應用程式由 IIS 裝載時，請確認應用程式已部署到 iis**管理員**的 [**基本設定**] 中所顯示的 iis**實體路徑**。
1. 藉由比較主控系統上的部署與專案 [*發行*] 資料夾的內容，確認已部署所有應用程式的檔案和資料夾。

如需已發佈 ASP.NET Core 應用程式佈建的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。 如需*web.config*檔案的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>。

### <a name="500-internal-server-error"></a>500 內部伺服器錯誤

應用程式啟動，但有錯誤導致伺服器無法完成要求。

此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。 回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」的形式出現。 「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。 從伺服器的觀點來看，這是正確的。 應用程式已啟動，但無法產生有效的回應。 在伺服器上的命令提示字元中執行應用程式，或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。

### <a name="5000-in-process-handler-load-failure"></a>500.0 同處理序處理常式載入失敗

背景工作處理序失敗。 應用程式未啟動。

載入[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)元件時發生未知的錯誤。 請採取下列其中一個動作：

* 連絡 [Microsoft 支援服務](https://support.microsoft.com/oas/default.aspx?prid=15832) (依序選取 [開發人員工具] 和 [ASP.NET Core])。
* 在 Stack Overflow 上詢問問題。
* 在我們的 [GitHub 存放庫](https://github.com/dotnet/AspNetCore)提出問題。

### <a name="50030-in-process-startup-failure"></a>500.30 同處理序啟動失敗

背景工作處理序失敗。 應用程式未啟動。

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動 .NET Core CLR 同進程，但無法啟動。 通常會從應用程式事件記錄檔中的專案和 ASP.NET Core 模組 stdout 記錄檔中判斷進程啟動失敗的原因。

常見的失敗狀況：

* 應用程式設定錯誤，因為以不存在的 ASP.NET Core 共用架構版本為目標。 請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。
* 使用 Azure Key Vault，缺少 Key Vault 的許可權。 檢查目標 Key Vault 中的存取原則，以確定已授與正確的許可權。

### <a name="50031-ancm-failed-to-find-native-dependencies"></a>500.31 ANCM 找不到原生相依性

背景工作處理序失敗。 應用程式未啟動。

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試在同進程中啟動 .net Core 執行時間，但無法啟動。 此啟動失敗的最常見原因是當 `Microsoft.NETCore.App` 或 `Microsoft.AspNetCore.App` 執行階段未安裝時。 如果應用程式部署至目標 ASP.NET Core 3.0，但電腦上無該版本，就會發生此錯誤。 範例錯誤訊息如下：

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

錯誤訊息會列出所有已安裝 .NET Core 版本和應用程式所要求的版本。 若要修正此錯誤，請使用以下其中一種方法：

* 在電腦上安裝適當的 .NET Core 版本。
* 將應用程式的目標 .NET Core 版本變更為電腦上版本。
* 將應用程式發佈為[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。

在開發過程中執行時 (`ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`)，特定的錯誤會寫入至 HTTP 回應。 在應用程式事件記錄檔中也可以找到進程啟動失敗的原因。

### <a name="50032-ancm-failed-to-load-dll"></a>500.32 ANCM 無法載入 dll

背景工作處理序失敗。 應用程式未啟動。

此錯誤最常見原因是針對不相容的處理器架構發佈應用程式。 如果背景工作處理序執行為 32 位元應用程式，而此應用程式已發佈至目標 64 位元，就會發生此錯誤。

若要修正此錯誤，請使用以下其中一種方法：

* 針對相同的處理器架構，將應用程式重新發佈為背景工作處理序。
* 將應用程式發佈為[架構相依部署](/dotnet/core/deploying/#framework-dependent-executables-fde)。

### <a name="50033-ancm-request-handler-load-failure"></a>500.33 ANCM 要求處理常式載入失敗

背景工作處理序失敗。 應用程式未啟動。

應用程式未參考 `Microsoft.AspNetCore.App` 架構。 只有以 `Microsoft.AspNetCore.App` 架構為目標的應用程式可以由[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主控。

若要修正這個錯誤，請確認應用程式以 `Microsoft.AspNetCore.App` 架構為目標。 檢查 `.runtimeconfig.json` 以驗證應用程式是否以該架構為目標。

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a>500.34 ANCM 不支援混合式裝載模型

背景工作處理序無法在相同的程序中執行同處理序應用程式和跨處理序應用程式。

若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a>500.35 ANCM 同一程序中有多個同處理序應用程式

工作者進程無法在同一個進程中執行多個同進程應用程式。

若要修正這個錯誤，請在不同的 IIS 應用程式集區中執行應用程式。

### <a name="50036-ancm-out-of-process-handler-load-failure"></a>500.36 ANCM 跨處理序處理常式載入失敗

跨處理序要求處理常式 *aspnetcorev2_outofprocess.dll* 不在 *aspnetcorev2.dll* 檔案旁邊。 這表示[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)安裝損毀。

若要修正這個錯誤，請修復 [.NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (適用於 IIS) 或 Visual Studio (適用於 IIS Express) 安裝。

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a>500.37 ANCM 無法在啟動時間限制內啟動

ANCM 無法在提供的啟動時間限制內啟動。 根據預設，逾時值為 120 秒。

在同一部電腦上啟動大量的應用程式時，就會發生此錯誤。 檢查伺服器在啟動期間是否出現 CPU/記憶體的使用量尖峰。 多個應用程式的啟動程序可能需要交錯進行。

### <a name="5025-process-failure"></a>502.5 處理序失敗

背景工作處理序失敗。 應用程式未啟動。

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。 通常會從應用程式事件記錄檔中的專案和 ASP.NET Core 模組 stdout 記錄檔中判斷進程啟動失敗的原因。

因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。 請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。 「共用的架構」是一組安裝於電腦上且由  *之類的中繼套件所參考的組件 (* .dll`Microsoft.AspNetCore.App` 檔案)。 中繼套件參考可以指定最低必要版本。 如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。

當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗] 錯誤頁面：

### <a name="failed-to-start-application-errorcode-0x800700c1"></a>無法啟動應用程式 (ErrorCode '0x800700c1')

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

應用程式無法啟動，因為無法載入應用程式的組件 ( *.dll*)。

當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。

確認應用程式集區的 32 位元設定正確：

1. 在 IIS 管理員的 [應用程式集區] 中，選取應用程式集區。
1. 在 [動作] 面板中，選取 [編輯應用程式集區] 下的 [進階設定]。
1. 設定 [啟用 32 位元應用程式]：
   * 如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。
   * 如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。

確認專案檔中的 `<Platform>` MSBuild 屬性和應用程式的已發佈位之間沒有衝突。

### <a name="connection-reset"></a>連線重設

如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」。 通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。 這類錯誤會在用戶端上顯示為「連線重設」錯誤。 [應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。

### <a name="default-startup-limits"></a>預設啟動限制

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是以120秒的預設*startupTimeLimit*來設定。 保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。 如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。

## <a name="troubleshoot-on-azure-app-service"></a>針對 Azure App Service 進行疑難排解

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a>應用程式事件記錄檔（Azure App Service）

若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題] 刀鋒視窗：

1. 在 Azure 入口網站的 [應用程式服務] 中，開啟應用程式。
1. 選取 [診斷並解決問題]。
1. 選取 [診斷工具] 標題。
1. 在 [支援工具] 下，選取 [應用程式事件] 按鈕。
1. 檢查 [來源] 資料行中 *IIS AspNetCoreModule* 或 **IIS AspNetCoreModule V2** 項目所提供的最新錯誤。

除了使用 [診斷並解決問題] 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：

1. 在 [開發工具] 區域中，開啟 [進階工具]。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 開啟 **LogFiles** 資料夾。
1. 選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。
1. 檢查記錄檔。 捲動至記錄檔的底部以查看最新事件。

### <a name="run-the-app-in-the-kudu-console"></a>在 Kudu 主控台中執行應用程式

許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。 您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：

1. 在 [開發工具] 區域中，開啟 [進階工具]。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。

#### <a name="test-a-32-bit-x86-app"></a>測試 32 位元 (x86) 應用程式

**目前的版本**

1. `cd d:\home\site\wwwroot`
1. 執行應用程式：
   * 如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * 如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：

     ```console
     {ASSEMBLY NAME}.exe
     ```

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

**在預覽版本上執行的 Framework 相依部署**

*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)
1. 執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

#### <a name="test-a-64-bit-x64-app"></a>測試 64 位元 (x64) 應用程式

**目前的版本**

* 如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：
  1. `cd D:\Program Files\dotnet`
  1. 執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`
* 如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：
  1. `cd D:\home\site\wwwroot`
  1. 執行應用程式：`{ASSEMBLY NAME}.exe`

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

**在預覽版本上執行的 Framework 相依部署**

*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)
1. 執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a>ASP.NET Core 模組 stdout 記錄檔（Azure App Service）

ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。 啟用及檢視 stdout 記錄檔：

1. 在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。
1. 在 [選取問題類別] 底下，選取 [Web 應用程式停止運作] 按鈕。
1. 在 **建議的解決方案** 底下 >**啟用 Stdout 記錄**檔重新導向，選取  按鈕以**開啟 Kudu 主控台 以編輯 web.config**。
1. 在 Kudu [診斷主控台] 中，將資料夾開啟至路徑 [site] > [wwwroot]。 向下捲動以顯露位於清單底部的 *web.config* 檔案。
1. 按一下 *web.config* 檔案旁邊的鉛筆圖示。
1. 將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。
1. 選取 [儲存] 以儲存已更新的 *web.config* 檔案。
1. 對應用程式發出要求。
1. 返回 Azure 入口網站。 選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 選取 **LogFiles** 資料夾。
1. 檢查 [修改時間] 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。
1. 當記錄檔開啟時，會顯示錯誤。

完成疑難排解時，請停用 stdout 記錄：

1. 在 Kudu [診斷主控台] 中，返回路徑 [site] > [wwwroot] 以顯露 *web.config* 檔案。 再次選取鉛筆圖示來開啟 **web.config** 檔案。
1. 將 **stdoutLogEnabled** 設定為 `false`。
1. 選取 [儲存] 以儲存檔案。

如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。 請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。
>
> 針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

### <a name="aspnet-core-module-debug-log-azure-app-service"></a>ASP.NET Core 模組的 debug 記錄檔（Azure App Service）

ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。 啟用及檢視 stdout 記錄檔：

1. 若要啟用增強型診斷記錄，請執行下列任一動作：
   * 遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。 重新部署應用程式。
   * 使用 Kudu 主控台，將`<handlerSettings>`增強型診斷記錄[中所顯示的 ](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) 新增至即時應用程式的 *web.config* 檔案：
     1. 在 [開發工具] 區域中，開啟 [進階工具]。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
     1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
     1. 將資料夾開啟至路徑 [site] > [wwwroot]。 選取鉛筆圖示來編輯 *web.config* 檔案。 新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。 選取 [儲存] 按鈕。
1. 在 [開發工具] 區域中，開啟 [進階工具]。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 將資料夾開啟至路徑 [site] > [wwwroot]。 如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。 如果已提供路徑，請巡覽至記錄檔的位置。
1. 使用檔案名稱旁的鉛筆圖示來開啟記錄檔。

完成疑難排解時，請停用偵錯記錄：

若要停用增強型偵錯記錄，請執行下列任一動作：

* 從 `<handlerSettings>`web.config*檔案本機移除* 並重新部署應用程式。
* 使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。 儲存檔案。

如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。

> [!WARNING]
> 無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。 記錄檔大小沒有任何限制。 請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。
>
> 針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

### <a name="slow-or-hanging-app-azure-app-service"></a>緩慢或懸掛應用程式（Azure App Service）

當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：

* [針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)

### <a name="monitoring-blades"></a>監視刀鋒視窗

監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。 這些刀鋒視窗可用來診斷 500 系列的錯誤。

請確認已安裝 ASP.NET Core 延伸模組。 如果未安裝這些延伸模組，請手動安裝：

1. 在 [開發工具] 刀鋒視窗區段中，選取 [延伸模組] 刀鋒視窗。
1. [ASP.NET Core 延伸模組] 應該會出現在清單中。
1. 如果未安裝這些延伸模組，請選取 [新增] 按鈕。
1. 從清單中選擇 [ASP.NET Core 延伸模組]。
1. 選取 [確定] 以接受法律條款。
1. 在 [新增延伸模組] 刀鋒視窗上，選取 [確定]。
1. 成功安裝延伸模組時，會有資訊快顯訊息提供指示。

如果未啟用 stdout 記錄，請依照下列步驟進行操作：

1. 在 Azure 入口網站中，選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 開啟路徑**網站**的資料夾 > **wwwroot**並向下滾動，以顯示清單底部*的 web.config 檔案*。
1. 按一下 *web.config* 檔案旁邊的鉛筆圖示。
1. 將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。
1. 選取 [儲存] 以儲存已更新的 *web.config* 檔案。

繼續接著啟用診斷記錄：

1. 在 Azure 入口網站中，選取 [診斷記錄檔] 刀鋒視窗。
1. 選取 [應用程式記錄 (檔案系統)] 和 [詳細錯誤訊息].的 [開啟] 開關。 選取刀鋒視窗頂端的 [儲存] 按鈕。
1. 若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤] 的 [開啟] 開關。
1. 選取 [記錄資料流] 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔] 刀鋒視窗之下)。
1. 對應用程式發出要求。
1. 在記錄資料流資料內，會指出錯誤的原因。

完成疑難排解時，請務必停用 stdout 記錄。

檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：

1. 在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。
1. 從資訊看板的 [支援工具] 區域中，選取 [失敗要求追蹤記錄檔]。

如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。

如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。
>
> 針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

## <a name="troubleshoot-on-iis"></a>針對 IIS 進行疑難排解

### <a name="application-event-log-iis"></a>應用程式事件記錄檔（IIS）

存取「應用程式事件記錄檔」：

1. 開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。
1. 在 [事件檢視器] 中，開啟 [Windows 記錄] 節點。
1. 選取 [應用程式] 以開啟「應用程式事件記錄檔」。
1. 搜尋與失敗應用程式相關的錯誤。 錯誤在 [來源] 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。

### <a name="run-the-app-at-a-command-prompt"></a>在命令提示字元中執行應用程式

許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。 您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。

#### <a name="framework-dependent-deployment"></a>與 Framework 相依的部署

如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：

1. 在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。 在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。
1. 來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。
1. 如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。 如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。 如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。

#### <a name="self-contained-deployment"></a>自封式部署

如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：

1. 在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。 在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。
1. 來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。
1. 如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。 如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。 如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。

### <a name="aspnet-core-module-stdout-log-iis"></a>ASP.NET Core 模組 stdout 記錄檔（IIS）

啟用及檢視 stdout 記錄檔：

1. 瀏覽至主控系統上網站的部署資料夾。
1. 如果 [logs] 資料夾不存在，請建立該資料夾。 如需有關如何讓 MSBuild 在部署中自動建立 [logs] 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。
1. 編輯 *web.config* 檔案。 將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs] 資料夾 (例如 `.\logs\stdout`)。 路徑中的 `stdout` 是記錄檔名稱前置詞。 建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。 使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。
1. 請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。
1. 儲存已更新的 *web.config* 檔案。
1. 對應用程式發出要求。
1. 瀏覽至 [logs] 資料夾。 尋找並開啟最新的 stdout 記錄檔。
1. 研究記錄檔以了解錯誤。

完成疑難排解時，請停用 stdout 記錄：

1. 編輯 *web.config* 檔案。
1. 將 **stdoutLogEnabled** 設定為 `false`。
1. 儲存檔案。

如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。
>
> 針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

### <a name="aspnet-core-module-debug-log-iis"></a>ASP.NET Core 模組的調試記錄檔（IIS）

將下列處理常式設定新增至應用程式*的 web.config 檔案*，以啟用 ASP.NET Core 模組的 debug 記錄檔：

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。

如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。

### <a name="enable-the-developer-exception-page"></a>啟用開發人員例外頁面

`ASPNETCORE_ENVIRONMENT`[環境變數可以新增至](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)web.config，以在開發環境中執行應用程式。 只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。

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

只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。 進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。 如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。

### <a name="obtain-data-from-an-app"></a>從應用程式取得資料

若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。 如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。

### <a name="slow-or-hanging-app-iis"></a>緩慢或懸掛應用程式（IIS）

損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。

#### <a name="app-crashes-or-encounters-an-exception"></a>應用程式損毀或發生例外狀況

從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：

1. 在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。 應用程式集區必須具備該資料夾的寫入權限。
1. 執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：
   * 如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * 如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. 在會導致損毀的情況下，執行應用程式。
1. 發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：
   * 如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：

     ```console
     .\DisableDumps w3wp.exe
     ```

   * 如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：

     ```console
     .\DisableDumps dotnet.exe
     ```

在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。 PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。

> [!WARNING]
> 損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>應用程式停止回應、在啟動期間失敗，或正常執行

當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。

#### <a name="analyze-the-dump"></a>分析傾印

您可以使用數種方法來分析傾印。 如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。

## <a name="clear-package-caches"></a>清除套件快取

升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。 在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。 大多數這些問題都可依照下列指示來進行修正：

1. 刪除 [bin] 和 [obj] 資料夾。
1. 從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。

   您也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成清除套件快取，並 `nuget locals all -clear`執行命令。 *nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。

1. 還原並重建專案。
1. 重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。

## <a name="additional-resources"></a>其他資源

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a>Azure 文件

* [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)
* [使用 Visual Studio 在 Azure App Service 中疑難排解 web 應用程式的遠端偵錯程式一節](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)
* [作法：監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)
* [使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Azure Web 應用程式的應用程式效能常見問題集](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)
* [Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a>Visual Studio 文件

* [Visual Studio 2017 中 Azure 上 IIS 的遠端 Debug ASP.NET Core](/visualstudio/debugger/remote-debugging-azure)
* [Visual Studio 2017 中遠端 IIS 電腦上的遠端 Debug ASP.NET Core](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [了解使用 Visual Studio 進行偵錯](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a>Visual Studio Code 檔

* [使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

本文提供有關一般應用程式啟動錯誤的資訊，以及如何在將應用程式部署至 Azure App Service 或 IIS 時診斷錯誤的指示：

[應用程式啟動錯誤](#app-startup-errors)  
說明常見的啟動 HTTP 狀態碼案例。

[針對 Azure App Service 進行疑難排解](#troubleshoot-on-azure-app-service)  
提供部署至 Azure App Service 之應用程式的疑難排解建議。

[針對 IIS 進行疑難排解](#troubleshoot-on-iis)  
提供部署至 IIS 或在本機 IIS Express 上執行之應用程式的疑難排解建議。 本指南適用于 Windows Server 和 Windows 桌面部署。

[清除套件快取](#clear-package-caches)  
說明當一致封裝在執行主要升級或變更封裝版本時，中斷應用程式時該怎麼辦。

[其他資源](#additional-resources)  
列出其他疑難排解主題。

## <a name="app-startup-errors"></a>應用程式啟動錯誤

在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。 使用本主題中的建議，可以診斷在本機進行偵錯工具時所發生的*502.5-進程失敗*或*500.30 啟動失敗*。

### <a name="40314-forbidden"></a>403.14 禁止

應用程式無法啟動。 會記錄下列錯誤：

```
The Web server is configured to not list the contents of this directory.
```

此錯誤通常是因為主控系統上的部署中斷所造成，其中包括下列任何一種情況：

* 應用程式會部署到裝載系統上錯誤的資料夾。
* 部署進程無法將應用程式的所有檔案和資料夾移到主控系統上的部署資料夾。
* 部署中遺漏*web.config*檔案，或*web.config*檔案內容的格式不正確。

執行下列步驟：

1. 刪除主控系統上部署資料夾中的所有檔案和資料夾。
1. 使用一般部署方法（例如 Visual Studio、PowerShell 或手動部署），將應用程式的 [*發佈*] 資料夾的內容重新部署至主機系統：
   * 請確認*web.config*檔案存在於部署中，而且其內容正確。
   * 在 Azure App Service 上裝載時，請確認應用程式已部署至 `D:\home\site\wwwroot` 資料夾。
   * 當應用程式由 IIS 裝載時，請確認應用程式已部署到 iis**管理員**的 [**基本設定**] 中所顯示的 iis**實體路徑**。
1. 藉由比較主控系統上的部署與專案 [*發行*] 資料夾的內容，確認已部署所有應用程式的檔案和資料夾。

如需已發佈 ASP.NET Core 應用程式佈建的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。 如需*web.config*檔案的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>。

### <a name="500-internal-server-error"></a>500 內部伺服器錯誤

應用程式啟動，但有錯誤導致伺服器無法完成要求。

此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。 回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」的形式出現。 「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。 從伺服器的觀點來看，這是正確的。 應用程式已啟動，但無法產生有效的回應。 在伺服器上的命令提示字元中執行應用程式，或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。

### <a name="5000-in-process-handler-load-failure"></a>500.0 同處理序處理常式載入失敗

背景工作處理序失敗。 應用程式未啟動。

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)找不到 .NET Core CLR，而且找不到同進程要求處理常式（*aspnetcorev2_inprocess .dll*）。 請確定：

* 應用程式以 [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet 套件或 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)為目標。
* 應用程式設為目標的 ASP.NET Core 共用架構版本有安裝在目標機器上。

### <a name="5000-out-of-process-handler-load-failure"></a>500.0 跨處理序處理常式載入失敗

背景工作處理序失敗。 應用程式未啟動。

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)找不到跨進程主控要求處理常式。 請確定 *aspnetcorev2_outofprocess.dll* 出現在子資料夾中，且位於 *aspnetcorev2.dll* 旁。

### <a name="5025-process-failure"></a>502.5 處理序失敗

背景工作處理序失敗。 應用程式未啟動。

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。 通常會從應用程式事件記錄檔中的專案和 ASP.NET Core 模組 stdout 記錄檔中判斷進程啟動失敗的原因。

因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。 請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。 「共用的架構」是一組安裝於電腦上且由  *之類的中繼套件所參考的組件 (* .dll`Microsoft.AspNetCore.App` 檔案)。 中繼套件參考可以指定最低必要版本。 如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。

當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗] 錯誤頁面：

### <a name="failed-to-start-application-errorcode-0x800700c1"></a>無法啟動應用程式 (ErrorCode '0x800700c1')

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

應用程式無法啟動，因為無法載入應用程式的組件 ( *.dll*)。

當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。

確認應用程式集區的 32 位元設定正確：

1. 在 IIS 管理員的 [應用程式集區] 中，選取應用程式集區。
1. 在 [動作] 面板中，選取 [編輯應用程式集區] 下的 [進階設定]。
1. 設定 [啟用 32 位元應用程式]：
   * 如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。
   * 如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。

確認專案檔中的 `<Platform>` MSBuild 屬性和應用程式的已發佈位之間沒有衝突。

### <a name="connection-reset"></a>連線重設

如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」。 通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。 這類錯誤會在用戶端上顯示為「連線重設」錯誤。 [應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。

### <a name="default-startup-limits"></a>預設啟動限制

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是以120秒的預設*startupTimeLimit*來設定。 保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。 如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。

## <a name="troubleshoot-on-azure-app-service"></a>針對 Azure App Service 進行疑難排解

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a>應用程式事件記錄檔（Azure App Service）

若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題] 刀鋒視窗：

1. 在 Azure 入口網站的 [應用程式服務] 中，開啟應用程式。
1. 選取 [診斷並解決問題]。
1. 選取 [診斷工具] 標題。
1. 在 [支援工具] 下，選取 [應用程式事件] 按鈕。
1. 檢查 [來源] 資料行中 *IIS AspNetCoreModule* 或 **IIS AspNetCoreModule V2** 項目所提供的最新錯誤。

除了使用 [診斷並解決問題] 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：

1. 在 [開發工具] 區域中，開啟 [進階工具]。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 開啟 **LogFiles** 資料夾。
1. 選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。
1. 檢查記錄檔。 捲動至記錄檔的底部以查看最新事件。

### <a name="run-the-app-in-the-kudu-console"></a>在 Kudu 主控台中執行應用程式

許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。 您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：

1. 在 [開發工具] 區域中，開啟 [進階工具]。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。

#### <a name="test-a-32-bit-x86-app"></a>測試 32 位元 (x86) 應用程式

**目前的版本**

1. `cd d:\home\site\wwwroot`
1. 執行應用程式：
   * 如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * 如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：

     ```console
     {ASSEMBLY NAME}.exe
     ```

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

**在預覽版本上執行的 Framework 相依部署**

*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)
1. 執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

#### <a name="test-a-64-bit-x64-app"></a>測試 64 位元 (x64) 應用程式

**目前的版本**

* 如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：
  1. `cd D:\Program Files\dotnet`
  1. 執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`
* 如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：
  1. `cd D:\home\site\wwwroot`
  1. 執行應用程式：`{ASSEMBLY NAME}.exe`

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

**在預覽版本上執行的 Framework 相依部署**

*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)
1. 執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a>ASP.NET Core 模組 stdout 記錄檔（Azure App Service）

ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。 啟用及檢視 stdout 記錄檔：

1. 在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。
1. 在 [選取問題類別] 底下，選取 [Web 應用程式停止運作] 按鈕。
1. 在 **建議的解決方案** 底下 >**啟用 Stdout 記錄**檔重新導向，選取  按鈕以**開啟 Kudu 主控台 以編輯 web.config**。
1. 在 Kudu [診斷主控台] 中，將資料夾開啟至路徑 [site] > [wwwroot]。 向下捲動以顯露位於清單底部的 *web.config* 檔案。
1. 按一下 *web.config* 檔案旁邊的鉛筆圖示。
1. 將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。
1. 選取 [儲存] 以儲存已更新的 *web.config* 檔案。
1. 對應用程式發出要求。
1. 返回 Azure 入口網站。 選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 選取 **LogFiles** 資料夾。
1. 檢查 [修改時間] 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。
1. 當記錄檔開啟時，會顯示錯誤。

完成疑難排解時，請停用 stdout 記錄：

1. 在 Kudu [診斷主控台] 中，返回路徑 [site] > [wwwroot] 以顯露 *web.config* 檔案。 再次選取鉛筆圖示來開啟 **web.config** 檔案。
1. 將 **stdoutLogEnabled** 設定為 `false`。
1. 選取 [儲存] 以儲存檔案。

如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。 請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。
>
> 針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

### <a name="aspnet-core-module-debug-log-azure-app-service"></a>ASP.NET Core 模組的 debug 記錄檔（Azure App Service）

ASP.NET Core 模組偵錯記錄提供 ASP.NET Core 模組中其他且更深入的記錄。 啟用及檢視 stdout 記錄檔：

1. 若要啟用增強型診斷記錄，請執行下列任一動作：
   * 遵循[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中的指示，來設定應用程式進行增強型診斷記錄。 重新部署應用程式。
   * 使用 Kudu 主控台，將`<handlerSettings>`增強型診斷記錄[中所顯示的 ](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) 新增至即時應用程式的 *web.config* 檔案：
     1. 在 [開發工具] 區域中，開啟 [進階工具]。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
     1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
     1. 將資料夾開啟至路徑 [site] > [wwwroot]。 選取鉛筆圖示來編輯 *web.config* 檔案。 新增 `<handlerSettings>` 區段，如[增強型診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)中所示。 選取 [儲存] 按鈕。
1. 在 [開發工具] 區域中，開啟 [進階工具]。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 將資料夾開啟至路徑 [site] > [wwwroot]。 如果未提供 *aspnetcore-debug.log* 檔案的路徑，該檔案會顯示在清單中。 如果已提供路徑，請巡覽至記錄檔的位置。
1. 使用檔案名稱旁的鉛筆圖示來開啟記錄檔。

完成疑難排解時，請停用偵錯記錄：

若要停用增強型偵錯記錄，請執行下列任一動作：

* 從 `<handlerSettings>`web.config*檔案本機移除* 並重新部署應用程式。
* 使用 Kudu 主控台來編輯 *web.config* 檔案並移除 `<handlerSettings>` 區段。 儲存檔案。

如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。

> [!WARNING]
> 無法停用偵錯記錄，可能會造成應用程式或伺服器發生失敗。 記錄檔大小沒有任何限制。 請只在針對應用程式啟動問題進行疑難排解時，才使用偵錯記錄。
>
> 針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

### <a name="slow-or-hanging-app-azure-app-service"></a>緩慢或懸掛應用程式（Azure App Service）

當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：

* [針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)

### <a name="monitoring-blades"></a>監視刀鋒視窗

監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。 這些刀鋒視窗可用來診斷 500 系列的錯誤。

請確認已安裝 ASP.NET Core 延伸模組。 如果未安裝這些延伸模組，請手動安裝：

1. 在 [開發工具] 刀鋒視窗區段中，選取 [延伸模組] 刀鋒視窗。
1. [ASP.NET Core 延伸模組] 應該會出現在清單中。
1. 如果未安裝這些延伸模組，請選取 [新增] 按鈕。
1. 從清單中選擇 [ASP.NET Core 延伸模組]。
1. 選取 [確定] 以接受法律條款。
1. 在 [新增延伸模組] 刀鋒視窗上，選取 [確定]。
1. 成功安裝延伸模組時，會有資訊快顯訊息提供指示。

如果未啟用 stdout 記錄，請依照下列步驟進行操作：

1. 在 Azure 入口網站中，選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 開啟路徑**網站**的資料夾 > **wwwroot**並向下滾動，以顯示清單底部*的 web.config 檔案*。
1. 按一下 *web.config* 檔案旁邊的鉛筆圖示。
1. 將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。
1. 選取 [儲存] 以儲存已更新的 *web.config* 檔案。

繼續接著啟用診斷記錄：

1. 在 Azure 入口網站中，選取 [診斷記錄檔] 刀鋒視窗。
1. 選取 [應用程式記錄 (檔案系統)] 和 [詳細錯誤訊息].的 [開啟] 開關。 選取刀鋒視窗頂端的 [儲存] 按鈕。
1. 若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤] 的 [開啟] 開關。
1. 選取 [記錄資料流] 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔] 刀鋒視窗之下)。
1. 對應用程式發出要求。
1. 在記錄資料流資料內，會指出錯誤的原因。

完成疑難排解時，請務必停用 stdout 記錄。

檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：

1. 在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。
1. 從資訊看板的 [支援工具] 區域中，選取 [失敗要求追蹤記錄檔]。

如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。

如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。
>
> 針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

## <a name="troubleshoot-on-iis"></a>針對 IIS 進行疑難排解

### <a name="application-event-log-iis"></a>應用程式事件記錄檔（IIS）

存取「應用程式事件記錄檔」：

1. 開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。
1. 在 [事件檢視器] 中，開啟 [Windows 記錄] 節點。
1. 選取 [應用程式] 以開啟「應用程式事件記錄檔」。
1. 搜尋與失敗應用程式相關的錯誤。 錯誤在 [來源] 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。

### <a name="run-the-app-at-a-command-prompt"></a>在命令提示字元中執行應用程式

許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。 您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。

#### <a name="framework-dependent-deployment"></a>與 Framework 相依的部署

如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：

1. 在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。 在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。
1. 來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。
1. 如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。 如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。 如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。

#### <a name="self-contained-deployment"></a>自封式部署

如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：

1. 在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。 在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。
1. 來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。
1. 如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。 如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。 如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。

### <a name="aspnet-core-module-stdout-log-iis"></a>ASP.NET Core 模組 stdout 記錄檔（IIS）

啟用及檢視 stdout 記錄檔：

1. 瀏覽至主控系統上網站的部署資料夾。
1. 如果 [logs] 資料夾不存在，請建立該資料夾。 如需有關如何讓 MSBuild 在部署中自動建立 [logs] 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。
1. 編輯 *web.config* 檔案。 將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs] 資料夾 (例如 `.\logs\stdout`)。 路徑中的 `stdout` 是記錄檔名稱前置詞。 建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。 使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。
1. 請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。
1. 儲存已更新的 *web.config* 檔案。
1. 對應用程式發出要求。
1. 瀏覽至 [logs] 資料夾。 尋找並開啟最新的 stdout 記錄檔。
1. 研究記錄檔以了解錯誤。

完成疑難排解時，請停用 stdout 記錄：

1. 編輯 *web.config* 檔案。
1. 將 **stdoutLogEnabled** 設定為 `false`。
1. 儲存檔案。

如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。
>
> 針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

### <a name="aspnet-core-module-debug-log-iis"></a>ASP.NET Core 模組的調試記錄檔（IIS）

將下列處理常式設定新增至應用程式*的 web.config 檔案*，以啟用 ASP.NET Core 模組的 debug 記錄檔：

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

確認為記錄指定的路徑存在，而且應用程式集區的身分識別具有該位置的寫入權限。

如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>。

### <a name="enable-the-developer-exception-page"></a>啟用開發人員例外頁面

`ASPNETCORE_ENVIRONMENT`[環境變數可以新增至](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)web.config，以在開發環境中執行應用程式。 只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。

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

只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。 進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。 如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。

### <a name="obtain-data-from-an-app"></a>從應用程式取得資料

若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。 如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。

### <a name="slow-or-hanging-app-iis"></a>緩慢或懸掛應用程式（IIS）

損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。

#### <a name="app-crashes-or-encounters-an-exception"></a>應用程式損毀或發生例外狀況

從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：

1. 在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。 應用程式集區必須具備該資料夾的寫入權限。
1. 執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：
   * 如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * 如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. 在會導致損毀的情況下，執行應用程式。
1. 發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：
   * 如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：

     ```console
     .\DisableDumps w3wp.exe
     ```

   * 如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：

     ```console
     .\DisableDumps dotnet.exe
     ```

在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。 PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。

> [!WARNING]
> 損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>應用程式停止回應、在啟動期間失敗，或正常執行

當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。

#### <a name="analyze-the-dump"></a>分析傾印

您可以使用數種方法來分析傾印。 如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。

## <a name="clear-package-caches"></a>清除套件快取

升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。 在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。 大多數這些問題都可依照下列指示來進行修正：

1. 刪除 [bin] 和 [obj] 資料夾。
1. 從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。

   您也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成清除套件快取，並 `nuget locals all -clear`執行命令。 *nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。

1. 還原並重建專案。
1. 重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。

## <a name="additional-resources"></a>其他資源

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a>Azure 文件

* [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)
* [使用 Visual Studio 在 Azure App Service 中疑難排解 web 應用程式的遠端偵錯程式一節](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)
* [作法：監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)
* [使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Azure Web 應用程式的應用程式效能常見問題集](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)
* [Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a>Visual Studio 文件

* [Visual Studio 2017 中 Azure 上 IIS 的遠端 Debug ASP.NET Core](/visualstudio/debugger/remote-debugging-azure)
* [Visual Studio 2017 中遠端 IIS 電腦上的遠端 Debug ASP.NET Core](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [了解使用 Visual Studio 進行偵錯](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a>Visual Studio Code 檔

* [使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

本文提供有關一般應用程式啟動錯誤的資訊，以及如何在將應用程式部署至 Azure App Service 或 IIS 時診斷錯誤的指示：

[應用程式啟動錯誤](#app-startup-errors)  
說明常見的啟動 HTTP 狀態碼案例。

[針對 Azure App Service 進行疑難排解](#troubleshoot-on-azure-app-service)  
提供部署至 Azure App Service 之應用程式的疑難排解建議。

[針對 IIS 進行疑難排解](#troubleshoot-on-iis)  
提供部署至 IIS 或在本機 IIS Express 上執行之應用程式的疑難排解建議。 本指南適用于 Windows Server 和 Windows 桌面部署。

[清除套件快取](#clear-package-caches)  
說明當一致封裝在執行主要升級或變更封裝版本時，中斷應用程式時該怎麼辦。

[其他資源](#additional-resources)  
列出其他疑難排解主題。

## <a name="app-startup-errors"></a>應用程式啟動錯誤

在 Visual Studio 中，進行偵錯時，ASP.NET Core 專案會預設為 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 裝載環境。 使用本主題中的建議，可以診斷本機上的偵錯工具發生的*502.5 進程失敗*。

### <a name="40314-forbidden"></a>403.14 禁止

應用程式無法啟動。 會記錄下列錯誤：

```
The Web server is configured to not list the contents of this directory.
```

此錯誤通常是因為主控系統上的部署中斷所造成，其中包括下列任何一種情況：

* 應用程式會部署到裝載系統上錯誤的資料夾。
* 部署進程無法將應用程式的所有檔案和資料夾移到主控系統上的部署資料夾。
* 部署中遺漏*web.config*檔案，或*web.config*檔案內容的格式不正確。

執行下列步驟：

1. 刪除主控系統上部署資料夾中的所有檔案和資料夾。
1. 使用一般部署方法（例如 Visual Studio、PowerShell 或手動部署），將應用程式的 [*發佈*] 資料夾的內容重新部署至主機系統：
   * 請確認*web.config*檔案存在於部署中，而且其內容正確。
   * 在 Azure App Service 上裝載時，請確認應用程式已部署至 `D:\home\site\wwwroot` 資料夾。
   * 當應用程式由 IIS 裝載時，請確認應用程式已部署到 iis**管理員**的 [**基本設定**] 中所顯示的 iis**實體路徑**。
1. 藉由比較主控系統上的部署與專案 [*發行*] 資料夾的內容，確認已部署所有應用程式的檔案和資料夾。

如需已發佈 ASP.NET Core 應用程式佈建的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。 如需*web.config*檔案的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>。

### <a name="500-internal-server-error"></a>500 內部伺服器錯誤

應用程式啟動，但有錯誤導致伺服器無法完成要求。

此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。 回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」的形式出現。 「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。 從伺服器的觀點來看，這是正確的。 應用程式已啟動，但無法產生有效的回應。 在伺服器上的命令提示字元中執行應用程式，或啟用 ASP.NET Core 模組 stdout 記錄檔，以針對問題進行疑難排解。

### <a name="5025-process-failure"></a>502.5 處理序失敗

背景工作處理序失敗。 應用程式未啟動。

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。 通常會從應用程式事件記錄檔中的專案和 ASP.NET Core 模組 stdout 記錄檔中判斷進程啟動失敗的原因。

因為目標 ASP.NET Core 共用架構的版本不存在，導致應用程式設定錯誤是常見的失敗狀況。 請檢查安裝在目標機器上的 ASP.NET Core 共用架構版本為何。 「共用的架構」是一組安裝於電腦上且由  *之類的中繼套件所參考的組件 (* .dll`Microsoft.AspNetCore.App` 檔案)。 中繼套件參考可以指定最低必要版本。 如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。

當裝載或應用程式設定錯誤造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗] 錯誤頁面：

### <a name="failed-to-start-application-errorcode-0x800700c1"></a>無法啟動應用程式 (ErrorCode '0x800700c1')

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

應用程式無法啟動，因為無法載入應用程式的組件 ( *.dll*)。

當已發行的應用程式與 w3wp/iisexpress 處理序之間出現位元不符的情況時，就會發生此錯誤。

確認應用程式集區的 32 位元設定正確：

1. 在 IIS 管理員的 [應用程式集區] 中，選取應用程式集區。
1. 在 [動作] 面板中，選取 [編輯應用程式集區] 下的 [進階設定]。
1. 設定 [啟用 32 位元應用程式]：
   * 如果部署 32 位元 (x86) 應用程式，請將值設定為 `True`。
   * 如果部署 64 位元 (x64) 應用程式，請將值設定為 `False`。

確認專案檔中的 `<Platform>` MSBuild 屬性和應用程式的已發佈位之間沒有衝突。

### <a name="connection-reset"></a>連線重設

如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」。 通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。 這類錯誤會在用戶端上顯示為「連線重設」錯誤。 [應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。

### <a name="default-startup-limits"></a>預設啟動限制

[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是以120秒的預設*startupTimeLimit*來設定。 保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。 如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。

## <a name="troubleshoot-on-azure-app-service"></a>針對 Azure App Service 進行疑難排解

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a>應用程式事件記錄檔（Azure App Service）

若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題] 刀鋒視窗：

1. 在 Azure 入口網站的 [應用程式服務] 中，開啟應用程式。
1. 選取 [診斷並解決問題]。
1. 選取 [診斷工具] 標題。
1. 在 [支援工具] 下，選取 [應用程式事件] 按鈕。
1. 檢查 [來源] 資料行中 *IIS AspNetCoreModule* 或 **IIS AspNetCoreModule V2** 項目所提供的最新錯誤。

除了使用 [診斷並解決問題] 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：

1. 在 [開發工具] 區域中，開啟 [進階工具]。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 開啟 **LogFiles** 資料夾。
1. 選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。
1. 檢查記錄檔。 捲動至記錄檔的底部以查看最新事件。

### <a name="run-the-app-in-the-kudu-console"></a>在 Kudu 主控台中執行應用程式

許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。 您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：

1. 在 [開發工具] 區域中，開啟 [進階工具]。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。

#### <a name="test-a-32-bit-x86-app"></a>測試 32 位元 (x86) 應用程式

**目前的版本**

1. `cd d:\home\site\wwwroot`
1. 執行應用程式：
   * 如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * 如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：

     ```console
     {ASSEMBLY NAME}.exe
     ```

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

**在預覽版本上執行的 Framework 相依部署**

*要求安裝 ASP.NET Core {VERSION} (x86) 執行階段網站延伸模組。*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` 是執行階段版本)
1. 執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

#### <a name="test-a-64-bit-x64-app"></a>測試 64 位元 (x64) 應用程式

**目前的版本**

* 如果應用程式是 64 位元 (x64) [架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：
  1. `cd D:\Program Files\dotnet`
  1. 執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`
* 如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：
  1. `cd D:\home\site\wwwroot`
  1. 執行應用程式：`{ASSEMBLY NAME}.exe`

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

**在預覽版本上執行的 Framework 相依部署**

*要求安裝 ASP.NET Core {VERSION} (x64) 執行階段網站延伸模組。*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` 是執行階段版本)
1. 執行應用程式：`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a>ASP.NET Core 模組 stdout 記錄檔（Azure App Service）

ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。 啟用及檢視 stdout 記錄檔：

1. 在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。
1. 在 [選取問題類別] 底下，選取 [Web 應用程式停止運作] 按鈕。
1. 在 **建議的解決方案** 底下 >**啟用 Stdout 記錄**檔重新導向，選取  按鈕以**開啟 Kudu 主控台 以編輯 web.config**。
1. 在 Kudu [診斷主控台] 中，將資料夾開啟至路徑 [site] > [wwwroot]。 向下捲動以顯露位於清單底部的 *web.config* 檔案。
1. 按一下 *web.config* 檔案旁邊的鉛筆圖示。
1. 將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。
1. 選取 [儲存] 以儲存已更新的 *web.config* 檔案。
1. 對應用程式發出要求。
1. 返回 Azure 入口網站。 選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 選取 **LogFiles** 資料夾。
1. 檢查 [修改時間] 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。
1. 當記錄檔開啟時，會顯示錯誤。

完成疑難排解時，請停用 stdout 記錄：

1. 在 Kudu [診斷主控台] 中，返回路徑 [site] > [wwwroot] 以顯露 *web.config* 檔案。 再次選取鉛筆圖示來開啟 **web.config** 檔案。
1. 將 **stdoutLogEnabled** 設定為 `false`。
1. 選取 [儲存] 以儲存檔案。

如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。 請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。
>
> 針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

### <a name="slow-or-hanging-app-azure-app-service"></a>緩慢或懸掛應用程式（Azure App Service）

當應用程式針對要求回應緩慢或無回應時，請參閱下列文章：

* [針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [使用損毀診斷程式網站延伸模組來擷取傾印，以取得 Azure Web 應用程式上的間歇性例外狀況或效能問題](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/) \(英文\)

### <a name="monitoring-blades"></a>監視刀鋒視窗

監視刀鋒視窗為本主題稍早所述的方法提供替代的疑難排解體驗。 這些刀鋒視窗可用來診斷 500 系列的錯誤。

請確認已安裝 ASP.NET Core 延伸模組。 如果未安裝這些延伸模組，請手動安裝：

1. 在 [開發工具] 刀鋒視窗區段中，選取 [延伸模組] 刀鋒視窗。
1. [ASP.NET Core 延伸模組] 應該會出現在清單中。
1. 如果未安裝這些延伸模組，請選取 [新增] 按鈕。
1. 從清單中選擇 [ASP.NET Core 延伸模組]。
1. 選取 [確定] 以接受法律條款。
1. 在 [新增延伸模組] 刀鋒視窗上，選取 [確定]。
1. 成功安裝延伸模組時，會有資訊快顯訊息提供指示。

如果未啟用 stdout 記錄，請依照下列步驟進行操作：

1. 在 Azure 入口網站中，選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。 選取 [執行 **]&rarr;** 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 開啟路徑**網站**的資料夾 > **wwwroot**並向下滾動，以顯示清單底部*的 web.config 檔案*。
1. 按一下 *web.config* 檔案旁邊的鉛筆圖示。
1. 將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。
1. 選取 [儲存] 以儲存已更新的 *web.config* 檔案。

繼續接著啟用診斷記錄：

1. 在 Azure 入口網站中，選取 [診斷記錄檔] 刀鋒視窗。
1. 選取 [應用程式記錄 (檔案系統)] 和 [詳細錯誤訊息].的 [開啟] 開關。 選取刀鋒視窗頂端的 [儲存] 按鈕。
1. 若要包含失敗要求追蹤 (也稱為「失敗要求事件緩衝處理」(FREB) 記錄)，請選取 [失敗要求的追蹤] 的 [開啟] 開關。
1. 選取 [記錄資料流] 刀鋒視窗 (列在入口網站中緊接在 [診斷記錄檔] 刀鋒視窗之下)。
1. 對應用程式發出要求。
1. 在記錄資料流資料內，會指出錯誤的原因。

完成疑難排解時，請務必停用 stdout 記錄。

檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：

1. 在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。
1. 從資訊看板的 [支援工具] 區域中，選取 [失敗要求追蹤記錄檔]。

如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。

如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。
>
> 針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

## <a name="troubleshoot-on-iis"></a>針對 IIS 進行疑難排解

### <a name="application-event-log-iis"></a>應用程式事件記錄檔（IIS）

存取「應用程式事件記錄檔」：

1. 開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。
1. 在 [事件檢視器] 中，開啟 [Windows 記錄] 節點。
1. 選取 [應用程式] 以開啟「應用程式事件記錄檔」。
1. 搜尋與失敗應用程式相關的錯誤。 錯誤在 [來源] 資料行中的值會是 *IIS AspNetCore Module* 或 *IIS Express AspNetCore Module*。

### <a name="run-the-app-at-a-command-prompt"></a>在命令提示字元中執行應用程式

許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。 您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。

#### <a name="framework-dependent-deployment"></a>與 Framework 相依的部署

如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：

1. 在命令提示字元中，瀏覽至部署資料夾，然後使用 *dotnet.exe* 來執行應用程式組件以執行應用程式。 在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`dotnet .\<assembly_name>.dll`。
1. 來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。
1. 如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。 如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。 如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。

#### <a name="self-contained-deployment"></a>自封式部署

如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)：

1. 在命令提示字元中，瀏覽至部署資料夾，然後執行應用程式的可執行檔。 在下列命令中，請以應用程式組件的名稱取代 \<assembly_name>：`<assembly_name>.exe`。
1. 來自應用程式的主控台輸出若有顯示任何錯誤，就會寫入至主控台視窗。
1. 如果是在對應用程式發出要求時發生錯誤，請對 Kestrel 進行接聽的主機和連接埠發出要求。 如果使用預設主機和連接埠，請對 `http://localhost:5000/` 發出要求。 如果應用程式在 Kestrel 端點位址正常回應，則問題與主機組態有關的機率較大，而與應用程式本身有關的機率較小。

### <a name="aspnet-core-module-stdout-log-iis"></a>ASP.NET Core 模組 stdout 記錄檔（IIS）

啟用及檢視 stdout 記錄檔：

1. 瀏覽至主控系統上網站的部署資料夾。
1. 如果 [logs] 資料夾不存在，請建立該資料夾。 如需有關如何讓 MSBuild 在部署中自動建立 [logs] 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。
1. 編輯 *web.config* 檔案。 將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為指向 [logs] 資料夾 (例如 `.\logs\stdout`)。 路徑中的 `stdout` 是記錄檔名稱前置詞。 建立記錄檔時，系統會自動新增時間戳記、處理序識別碼及副檔名。 使用 `stdout` 作為檔案名稱前置詞時，一般記錄檔會命名為 *stdout_20180205184032_5412.log*。
1. 請確定您的應用程式集區身分識別具有 *logs* 資料夾的寫入權限。
1. 儲存已更新的 *web.config* 檔案。
1. 對應用程式發出要求。
1. 瀏覽至 [logs] 資料夾。 尋找並開啟最新的 stdout 記錄檔。
1. 研究記錄檔以了解錯誤。

完成疑難排解時，請停用 stdout 記錄：

1. 編輯 *web.config* 檔案。
1. 將 **stdoutLogEnabled** 設定為 `false`。
1. 儲存檔案。

如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。
>
> 針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

### <a name="enable-the-developer-exception-page"></a>啟用開發人員例外頁面

`ASPNETCORE_ENVIRONMENT`[環境變數可以新增至](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)web.config，以在開發環境中執行應用程式。 只要主機產生器上的 `UseEnvironment` 不會覆寫應用程式啟動內的環境，設定該環境變數便可允許在應用程式執行時顯示[開發人員例外狀況頁面](xref:fundamentals/error-handling)。

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

只有在沒有對網際網路公開的暫存和測試伺服器上使用時，才建議為 `ASPNETCORE_ENVIRONMENT` 設定環境變數。 進行疑難排解之後，請從 *web.config* 檔案中移除環境變數。 如需有關在 *web.config* 中設定環境變數的資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)。

### <a name="obtain-data-from-an-app"></a>從應用程式取得資料

若應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。 如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。

### <a name="slow-or-hanging-app-iis"></a>緩慢或懸掛應用程式（IIS）

損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。

#### <a name="app-crashes-or-encounters-an-exception"></a>應用程式損毀或發生例外狀況

從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：

1. 在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。 應用程式集區必須具備該資料夾的寫入權限。
1. 執行 [EnableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)：
   * 如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * 如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. 在會導致損毀的情況下，執行應用程式。
1. 發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)：
   * 如果應用程式是使用[同處理序主控模型](xref:host-and-deploy/iis/index#in-process-hosting-model)，請執行 *w3wp.exe* 的指令碼：

     ```console
     .\DisableDumps w3wp.exe
     ```

   * 如果應用程式是使用[跨處理序主控模型](xref:host-and-deploy/iis/index#out-of-process-hosting-model)，請執行 *dotnet.exe* 的指令碼：

     ```console
     .\DisableDumps dotnet.exe
     ```

在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。 PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。

> [!WARNING]
> 損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>應用程式停止回應、在啟動期間失敗，或正常執行

當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。

#### <a name="analyze-the-dump"></a>分析傾印

您可以使用數種方法來分析傾印。 如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。

## <a name="clear-package-caches"></a>清除套件快取

升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。 在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。 大多數這些問題都可依照下列指示來進行修正：

1. 刪除 [bin] 和 [obj] 資料夾。
1. 從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。

   您也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成清除套件快取，並 `nuget locals all -clear`執行命令。 *nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。

1. 還原並重建專案。
1. 重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。

## <a name="additional-resources"></a>其他資源

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a>Azure 文件

* [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)
* [使用 Visual Studio 在 Azure App Service 中疑難排解 web 應用程式的遠端偵錯程式一節](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)
* [作法：監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)
* [使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Azure Web 應用程式的應用程式效能常見問題集](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)
* [Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a>Visual Studio 文件

* [Visual Studio 2017 中 Azure 上 IIS 的遠端 Debug ASP.NET Core](/visualstudio/debugger/remote-debugging-azure)
* [Visual Studio 2017 中遠端 IIS 電腦上的遠端 Debug ASP.NET Core](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [了解使用 Visual Studio 進行偵錯](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a>Visual Studio Code 檔

* [使用 Visual Studio Code 進行偵錯](https://code.visualstudio.com/docs/editor/debugging) \(英文\)

::: moniker-end
