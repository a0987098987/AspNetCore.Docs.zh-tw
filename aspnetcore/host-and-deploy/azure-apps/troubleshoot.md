---
title: "疑難排解 Azure App Service 上的 ASP.NET Core"
author: guardrex
description: "了解如何診斷 ASP.NET Core Azure App Service 部署的問題。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 27a46446e9bf63e96eecc392e6d6863e27b34730
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>疑難排解 Azure App Service 上的 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex)

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

本文說明如何診斷 ASP.NET Core 應用程式使用 Azure 應用程式服務的診斷工具的啟動問題。 如需其他疑難排解建議，請參閱[Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)和[如何： 監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)Azure 文件中。

## <a name="app-startup-errors"></a>應用程式啟動錯誤

**502.5 處理程序失敗**  
工作者處理序會失敗。 無法啟動應用程式。

[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)嘗試啟動背景工作處理序，但它無法啟動。 經常檢查應用程式事件記錄檔可協助疑難排解這個問題類型。 存取記錄檔中會說明[應用程式事件記錄檔](#application-event-log)> 一節。

*502.5 的處理序失敗*設定不正確的應用程式造成工作者處理序失敗時傳回錯誤頁面：

![瀏覽器視窗中顯示 502.5 處理序失敗的頁面](troubleshoot/_static/process-failure-page.png)

**500 內部伺服器錯誤**  
應用程式啟動，但因為錯誤而造成伺服器無法完成要求。

在啟動期間，或建立回應時，應用程式的程式碼中會發生此錯誤。 回應可能會包含任何內容，或回應可能會顯示為*500 內部伺服器錯誤*瀏覽器中。 應用程式事件記錄檔通常會指出應用程式正常啟動。 從伺服器的觀點來看，這是正確。 應用程式沒有開始，但它無法產生有效的回應。 [Kudu 主控台中執行應用程式](#run-the-app-in-the-kudu-console)或[啟用 ASP.NET 核心模組 stdout 記錄](#aspnet-core-module-stdout-log)來疑難排解問題。

**連接重設**

如果會傳送標頭之後，就會發生錯誤，則晚伺服器傳送**500 內部伺服器錯誤**發生的錯誤。 這通常發生在回應複雜物件的序列化期間發生錯誤時。 這種類型的錯誤會顯示為*連接重設*用戶端上的錯誤。 [應用程式記錄](xref:fundamentals/logging/index)可協助疑難排解這些類型的錯誤。

## <a name="default-startup-limits"></a>預設啟動限制

預設值設定 ASP.NET 核心模組*startupTimeLimit*為 120 秒。 當保留預設值，應用程式可能需要兩分鐘的時間啟動之前模組記錄處理序失敗。 如需設定此模組的資訊，請參閱[aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。

## <a name="troubleshoot-app-startup-errors"></a>疑難排解應用程式啟動錯誤

### <a name="application-event-log"></a>應用程式事件記錄檔

若要存取應用程式事件記錄檔，請使用**診斷並解決問題**刀鋒視窗，在 Azure 入口網站：

1. 在 Azure 入口網站中，開啟 [應用程式] 刀鋒視窗中的**應用程式服務**刀鋒視窗。
1. 選取**診斷並解決問題**刀鋒視窗。
1. 在下**選取問題類別**，選取**Web 應用程式向** 按鈕。
1. 在下**建議的解決方案**，開啟的窗格**開啟應用程式事件記錄檔**。 選取**開啟應用程式事件記錄檔** 按鈕。
1. 檢查所提供的最新錯誤*IIS AspNetCoreModule*中**來源**資料行。

除了使用**診斷並解決問題**刀鋒視窗會檢查直接使用的應用程式事件記錄檔[Kudu](https://github.com/projectkudu/kudu/wiki):

1. 選取**進階工具**刀鋒視窗中的**開發工具**區域。 選取**移&rarr;**  按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。
1. 開啟**LogFiles**資料夾。
1. 選取鉛筆圖示旁*eventlog.xml*檔案。
1. 檢查記錄檔。 捲動至底部，以查看最新的事件記錄檔。

### <a name="run-the-app-in-the-kudu-console"></a>Kudu 主控台中執行應用程式

許多的啟動錯誤不會產生應用程式事件日誌中的有用資訊。 您可以在執行應用程式[Kudu](https://github.com/projectkudu/kudu/wiki)發現錯誤的遠端執行主控台：

1. 選取**進階工具**刀鋒視窗中的**開發工具**區域。 選取**移&rarr;**  按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。
1. 開啟資料夾的路徑**網站** > **wwwroot**。
1. 在主控台中，執行應用程式藉由執行應用程式的組件。
   * 如果應用程式是[framework 相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，執行應用程式的組件*dotnet.exe*。 下列命令，以取代應用程式的組件名稱`<assembly_name>`: `dotnet .\<assembly_name>.dll`
   * 如果應用程式是[獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd)，請執行應用程式的可執行檔。 下列命令，以取代應用程式的組件名稱`<assembly_name>`: `<assembly_name>.exe`
1. 主控台應用程式，並顯示任何錯誤，從輸出送到 Kudu 主控台。

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET 核心模組 stdout 記錄檔

ASP.NET 核心模組 stdout 記錄通常會記錄應用程式事件日誌中找不到有用的錯誤訊息。 若要啟用，並檢視 stdout 記錄檔：

1. 瀏覽至**診斷並解決問題**在 Azure 入口網站的刀鋒視窗。
1. 在下**選取問題類別**，選取**Web 應用程式向** 按鈕。
1. 在下**建議的解決方案** > **啟用 Stdout 記錄檔重新導向**，選取按鈕以**開啟 Kudu 主控台編輯 Web.Config**。
1. 在 Kudu**診斷主控台**，開啟的資料夾路徑**網站** > **wwwroot**。 向下捲動至顯示*web.config*檔案清單的底部。
1. 按一下鉛筆圖示旁*web.config*檔案。
1. 設定**stdoutLogEnabled**至`true`並變更**stdoutLogFile**路徑： `\\?\%home%\LogFiles\stdout`。
1. 選取**儲存**來儲存已更新*web.config*檔案。
1. 應用程式提出要求。
1. 返回 Azure 入口網站。 選取**進階工具**刀鋒視窗中的**開發工具**區域。 選取**移&rarr;**  按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。
1. 選取**LogFiles**資料夾。
1. 檢查**Modified**資料行，然後選取鉛筆圖示，以編輯 stdout 記錄的最新的修改日期。
1. 當記錄檔開啟時，會顯示錯誤。

**重要！** 停用 stdout 記錄完成疑難排解時。

1. 在 Kudu**診斷主控台**，返回路徑**網站** > **wwwroot**以顯示*web.config*檔案。 開啟**web.config**檔案一次選取鉛筆圖示。
1. 設定**stdoutLogEnabled**至`false`。
1. 選取**儲存**儲存檔案。

> [!WARNING]
> 若要停用 stdout 記錄的失敗可能會導致應用程式或伺服器失敗。 因為它並沒有記錄檔大小或數量上的限制。
>
> 例行記錄中的 ASP.NET Core 應用程式、 使用限制記錄檔大小，並且會旋轉記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

## <a name="common-startup-errors"></a>常見的啟動錯誤 

請參閱[ASP.NET Core 常見錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)。 參考主題會說明最常見的問題阻礙應用程式啟動。

## <a name="slow-or-hanging-app"></a>較慢或無回應的應用程式

當應用程式回應變慢或停止回應的要求時，請參閱[疑難排解緩慢的 web 應用程式在 Azure App Service 中的效能問題](/azure/app-service/app-service-web-troubleshoot-performance-degradation)偵錯指引。

## <a name="remote-debugging"></a>遠端偵錯

請參閱下列主題：

* [遠端偵錯 web 應用程式的疑難排解中使用 Visual Studio 的 Azure App Service web 應用程式 區段](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)（Azure 文件）
* [遠端偵錯 ASP.NET Core 上 IIS 中的 Azure in Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) （Visual Studio 文件）

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/)提供裝載於 Azure 應用程式服務，包括錯誤記錄和報告功能的應用程式的遙測。 Application Insights 只可以在應用程式啟動時的應用程式記錄功能會變成可用之後，會發生的錯誤報告。 如需詳細資訊，請參閱[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)。

## <a name="monitoring-blades"></a>監視刀鋒視窗

監視刀鋒提供疑難排解體驗，本主題稍早所述之方法的替代方案。 可用來診斷 500 系列錯誤設定了這些刀鋒視窗。

確認已安裝 ASP.NET Core 擴充功能。 如果尚未安裝擴充功能，請手動進行安裝：

1. 在**開發工具**刀鋒視窗區段中，選取**延伸**刀鋒視窗。
1. **ASP.NET Core Extensions**應該會出現在清單中。
1. 如果尚未安裝擴充功能，請選取**新增** 按鈕。
1. 選擇**ASP.NET Core Extensions**從清單中。
1. 選取**確定**接受法律合約。
1. 選取**確定**上**將延伸加入**刀鋒視窗。
1. 資訊的快顯訊息表示已成功安裝擴充功能時。

如果未啟用 stdout 記錄，請遵循下列步驟：

1. 在 Azure 入口網站中，選取**進階工具**刀鋒視窗中的**開發工具**區域。 選取**移&rarr;**  按鈕。 Kudu 主控台會在新的瀏覽器索引標籤或視窗中開啟。
1. 使用導覽列頂端的頁面上，開啟**偵錯主控台**選取**CMD**。
1. 開啟資料夾的路徑**網站** > **wwwroot**和向下捲動至顯示*web.config*檔案清單的底部。
1. 按一下鉛筆圖示旁*web.config*檔案。
1. 設定**stdoutLogEnabled**至`true`並變更**stdoutLogFile**路徑： `\\?\%home%\LogFiles\stdout`。
1. 選取**儲存**來儲存已更新*web.config*檔案。

若要啟用診斷記錄，繼續進行：

1. 在 Azure 入口網站中，選取**診斷記錄檔**刀鋒視窗。
1. 選取**上**切換**的應用程式記錄 （檔案系統）**和**詳細錯誤訊息**。 選取**儲存**刀鋒視窗頂端的按鈕。
1. 若要包含失敗的要求追蹤，也就是失敗的要求事件緩衝處理 (FREB) 記錄中，選取**上**切換**追蹤失敗的要求**。 
1. 選取**記錄檔資料流**刀鋒視窗，立即會列在底下**診斷記錄檔**在入口網站的刀鋒視窗。
1. 應用程式提出要求。
1. 在記錄檔之資料流資料，會指出錯誤的原因。

**重要！** 請務必停用 stdout 記錄完成疑難排解時。 中的指示，請參閱 < [ASP.NET 核心模組 stdout 記錄](#aspnet-core-module-stdout-log)> 一節。

若要檢視失敗的要求追蹤記錄檔 （FREB 記錄檔）：

1. 瀏覽至**診斷並解決問題**在 Azure 入口網站的刀鋒視窗。
1. 選取**失敗的要求追蹤記錄**從**支援工具**區域的 [資訊看板]。

請參閱[失敗要求追蹤的 Azure App Service 主題中的 web 應用程式啟用診斷記錄 區段](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces)和[在 Azure 中的 Web 應用程式的應用程式效能常見問題集： 如何開啟追蹤失敗的要求？](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing)如需詳細資訊。

如需詳細資訊，請參閱[啟用 Azure App Service 中的 web 應用程式的診斷記錄](/azure/app-service/web-sites-enable-diagnostic-log)。

> [!WARNING]
> 若要停用 stdout 記錄的失敗可能會導致應用程式或伺服器失敗。 因為它並沒有記錄檔大小或數量上的限制。
>
> 例行記錄中的 ASP.NET Core 應用程式、 使用限制記錄檔大小，並且會旋轉記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

## <a name="additional-resources"></a>其他資源

* [ASP.NET Core 中的錯誤處理簡介](xref:fundamentals/error-handling)
* [Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)
* [疑難排解 Azure App Service 使用 Visual Studio 中的 web 應用程式](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [疑難排解 「 502 不正確的閘道 」 和 「 503 服務無法使用 「 Azure web 應用程式中的 HTTP 錯誤](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Azure App Service 中的速度慢的 web 應用程式效能問題的疑難排解](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [在 Azure 中的 Web 應用程式的應用程式效能常見問題集](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
