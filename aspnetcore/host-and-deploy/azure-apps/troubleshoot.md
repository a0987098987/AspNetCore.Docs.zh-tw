---
title: 針對 Azure App Service 上的 ASP.NET Core 進行疑難排解
author: guardrex
description: 了解如何診斷 ASP.NET Core Azure App Service 部署的問題。
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: a995c743b4e43be8bea5329affb3f2c736b1d016
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902550"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>針對 Azure App Service 上的 ASP.NET Core 進行疑難排解

作者：[Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

本文說明如何使用 Azure App Service 診斷工具來診斷 ASP.NET Core 應用程式啟動問題。 如需其他疑難排解建議，請參閱 Azure 文件中的 [Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)和[做法：監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)。

## <a name="app-startup-errors"></a>應用程式啟動錯誤

**502.5 處理序失敗**  
背景工作處理序失敗。 應用程式未啟動。

[ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)嘗試啟動背景工作處理序，但無法啟動。 檢查「應用程式事件記錄檔」通常有助於針對這類問題進行疑難排解。 [應用程式事件記錄檔](#application-event-log)一節說明了如何存取此記錄檔。

當設定錯誤的應用程式造成背景工作處理序發生失敗時，會傳回 [502.5 處理序失敗] 錯誤頁面：

![顯示 [502.5 處理序失敗] 頁面的瀏覽器視窗](troubleshoot/_static/process-failure-page.png)

**500 內部伺服器錯誤**  
應用程式啟動，但有錯誤導致伺服器無法完成要求。

此錯誤是在啟動或建立回應時，在應用程式的程式碼內發生。 回應可能未包含任何內容，或是回應可能在瀏覽器中以「500 內部伺服器錯誤」的形式出現。 「應用程式事件記錄檔」通常會指出該應用程式已正常啟動。 從伺服器的觀點來看，這是正確的。 應用程式已啟動，但無法產生有效的回應。 請[在 Kudu 主控台中執行應用程式](#run-the-app-in-the-kudu-console)或[啟用 ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)，以針對問題進行疑難排解。

**連線重設**

如果是在傳送標頭之後才發生錯誤，則當發生錯誤時，伺服器已來不及傳送「500 內部伺服器錯誤」。 通常是在將回應的複雜物件序列化的期間發生錯誤時，會發生此錯誤。 這類錯誤會在用戶端上顯示為「連線重設」錯誤。 [應用程式記錄](xref:fundamentals/logging/index)可協助針對這些類型的錯誤進行疑難排解。

## <a name="default-startup-limits"></a>預設啟動限制

ASP.NET Core 模組上已設定預設的 *startupTimeLimit* 120 秒。 保留預設值時，在模組記錄處理序失敗之前，應用程式最多可花費兩分鐘來進行啟動。 如需有關設定模組的資訊，請參閱 [aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。

## <a name="troubleshoot-app-startup-errors"></a>針對應用程式啟動錯誤進行疑難排解

### <a name="application-event-log"></a>應用程式事件記錄檔

若要存取「應用程式事件記錄檔」，請使用 Azure 入口網站中的 [診斷並解決問題] 刀鋒視窗：

1. 在 Azure 入口網站的 [應用程式服務] 刀鋒視窗中，開啟應用程式的刀鋒視窗。
1. 選取 [診斷並解決問題] 刀鋒視窗。
1. 在 [選取問題類別] 底下，選取 [Web 應用程式停止運作] 按鈕。
1. 在 [建議的解決方案] 底下，開啟用來**開啟應用程式事件記錄檔**的窗格。 選取 [開啟應用程式事件記錄檔] 按鈕。
1. 檢查 [來源] 資料行中 *IIS AspNetCoreModule* 所提供的最新錯誤。

除了使用 [診斷並解決問題] 刀鋒視窗之外，也可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 來直接檢查「應用程式事件記錄檔」：

1. 選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。 選取 [執行&rarr;] 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 開啟 [LogFiles] 資料夾。
1. 選取 *eventlog.xml* 檔案旁邊的鉛筆圖示。
1. 檢查記錄檔。 捲動至記錄檔的底部以查看最新事件。

### <a name="run-the-app-in-the-kudu-console"></a>在 Kudu 主控台中執行應用程式

許多啟動錯誤不會在「應用程式事件記錄檔」中產生實用的資訊。 您可以在 [Kudu](https://github.com/projectkudu/kudu/wiki)「遠端執行主控台」中執行應用程式來探索錯誤：

1. 選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。 選取 [執行&rarr;] 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 將資料夾開啟至路徑 [site] > [wwwroot]。
1. 在主控台中，藉由執行應用程式的組件來執行應用程式。
   * 如果應用程式是[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請使用 *dotnet.exe* 來執行應用程式的組件。 在下列命令中，請以應用程式組件的名稱取代 `<assembly_name>`：`dotnet .\<assembly_name>.dll`
   * 如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，請執行應用程式的可執行檔。 在下列命令中，請以應用程式組件的名稱取代 `<assembly_name>`：`<assembly_name>.exe`
1. 來自應用程式的主控台輸出若有顯示任何錯誤，就會透過管道傳送給 Kudu 主控台。

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core 模組 stdout 記錄檔

ASP.NET Core 模組 stdout 記錄檔通常會記錄「應用程式事件記錄檔」中所沒有的實用訊息。 啟用及檢視 stdout 記錄檔：

1. 在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。
1. 在 [選取問題類別] 底下，選取 [Web 應用程式停止運作] 按鈕。
1. 在 [建議的解決方案] > [啟用 Stdout 記錄檔重新導向] 底下，選取用來**開啟 Kudu 主控台以編輯 Web.Config** 的按鈕。
1. 在 Kudu [診斷主控台] 中，將資料夾開啟至路徑 [site] > [wwwroot]。 向下捲動以顯露位於清單底部的 *web.config* 檔案。
1. 按一下 *web.config* 檔案旁邊的鉛筆圖示。
1. 將 **stdoutLogEnabled** 設定為 `true`，並將 **stdoutLogFile** 路徑變更為：`\\?\%home%\LogFiles\stdout`。
1. 選取 [儲存] 以儲存已更新的 *web.config* 檔案。
1. 對應用程式發出要求。
1. 返回 Azure 入口網站。 選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。 選取 [執行&rarr;] 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 選取 [LogFiles] 資料夾。
1. 檢查 [修改時間] 資料行，然後選取鉛筆圖示來編輯修改日期最新的 stdout 記錄檔。
1. 當記錄檔開啟時，會顯示錯誤。

**重要！** 完成疑難排解時，請停用 stdout 記錄。

1. 在 Kudu [診斷主控台] 中，返回路徑 [site] > [wwwroot] 以顯露 *web.config* 檔案。 再次選取鉛筆圖示來開啟 **web.config** 檔案。
1. 將 **stdoutLogEnabled** 設定為 `false`。
1. 選取 [儲存] 以儲存檔案。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。 請只在針對應用程式啟動問題進行疑難排解時，才使用 stdout 記錄。
>
> 針對 ASP.NET Core 應用程式啟動後的一般記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

## <a name="common-startup-errors"></a>常見的啟動錯誤 

請參閱 [ASP.NET Core 常見錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)。 參考主題涵蓋了大多數導致應用程式無法啟動的常見問題。

## <a name="slow-or-hanging-app"></a>回應緩慢或無回應的應用程式

當應用程式針對要求回應緩慢或無回應時，請參閱[針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解](/azure/app-service/app-service-web-troubleshoot-performance-degradation)來取得疑難排解指南。

## <a name="remote-debugging"></a>遠端偵錯

請參閱下列主題：

* [＜使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式＞的＜遠端偵錯 Web 應用程式＞一節](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (Azure 文件)
* [在 Visual Studio 2017 中針對 Azure 中 IIS 上的 ASP.NET Core 進行遠端偵錯](/visualstudio/debugger/remote-debugging-azure) \(機器翻譯\) (Visual Studio 文件)

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) 提供來自 Azure App Service 中所裝載應用程式的遙測資料，包括錯誤記錄和報告功能。 Application Insights 只能針對在應用程式啟動之後，應用程式記錄功能變成可用時所發生的錯誤提出報告。 如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。

## <a name="monitoring-blades"></a>監視刀鋒視窗

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

1. 在 Azure 入口網站中，選取 [開發工具] 區域中的 [進階工具] 刀鋒視窗。 選取 [執行&rarr;] 按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用頁面頂端的導覽列，開啟 [偵錯主控台]，然後選取 [CMD]。
1. 將資料夾開啟至路徑 [site] > [wwwroot]，然後向下捲動以顯露位於清單底部的 *web.config* 檔案。
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

**重要！** 完成疑難排解時，請務必停用 stdout 記錄。 請參閱 [ASP.NET Core 模組 stdout 記錄檔](#aspnet-core-module-stdout-log)一節中的指示。

檢視失敗要求追蹤記錄檔 (FREB 記錄檔)：

1. 在 Azure 入口網站中，瀏覽至 [診斷並解決問題] 刀鋒視窗。
1. 從資訊看板的 [支援工具] 區域中，選取 [失敗要求追蹤記錄檔]。

如需詳細資訊，請參閱[＜在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能＞主題的＜失敗要求追蹤＞一節](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[Azure Web 應用程式的應用程式效能常見問題集：如何開啟失敗要求追蹤？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)。

如需詳細資訊，請參閱[在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)。

> [!WARNING]
> 如果無法停用 stdout 記錄檔，可能會造成應用程式或伺服器發生失敗。 因為它並沒有記錄檔大小或數量上的限制。
>
> 針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

## <a name="additional-resources"></a>其他資源

* [ASP.NET Core 中的錯誤處理簡介](xref:fundamentals/error-handling)
* [Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)
* [使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [對您的 Azure Web 應用程式中「502 不正確的閘道」和「503 服務無法使用」的 HTTP 錯誤進行疑難排解](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [針對 Azure App Service 中 Web 應用程式效能變慢的問題進行疑難排解](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Azure Web 應用程式的應用程式效能常見問題集](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web 應用程式沙箱 (App Service 執行階段執行限制)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) \(英文\)
* [Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
