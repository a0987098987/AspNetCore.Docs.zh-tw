---
title: 使用 ASP.NET Core 和 Azure 的 DevOps |監視和偵錯
author: CamSoper
description: 本指南為如何為 Azure 上裝載的 ASP.NET Core 應用程式，建置 DevOps 管線的完整指導。
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/monitor
ms.openlocfilehash: c4013de574fdf34114f2ae6c6a2150d72f807578
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090178"
---
# <a name="monitor-and-debug"></a>監視和偵錯

部署應用程式和內建的 DevOps 管線，請務必了解如何監視和疑難排解應用程式。

在本節中，您將完成下列工作：

* 尋找基本的監視和疑難排解在 Azure 入口網站中的資料
* 了解 Azure 監視器的所有 Azure 服務所提供的更深入的了解計量
* 使用 Application Insights 時使用的 web 應用程式連線的應用程式程式碼剖析
* 開啟記錄功能，並了解如何下載記錄檔
* Stream 即時記錄檔
* 了解如何設定警示
* 深入了解遠端偵錯 Azure App Service web apps。

## <a name="basic-monitoring-and-troubleshooting"></a>基本監視和疑難排解

App Service web 應用程式輕鬆地即時監視。 Azure 入口網站將呈現簡單易懂的圖表和圖形中的計量。

1. 開啟[Azure 入口網站](https://portal.azure.com)，然後瀏覽至*mywebapp\<unique_number\>*  App Service。

1. **概觀**索引標籤會顯示有用的 「 在摘要 」 資訊，包括顯示最近使用的計量的圖表。

    ![概觀面板](./media/monitoring/overview.png)

    * **Http 5xx**： 伺服器端錯誤計數，通常是 ASP.NET Core 程式碼中的例外狀況。
    * **資料輸入**： 進入您的 web 應用程式的資料輸入。
    * **資料輸出**： 從您的 web 應用程式輸出至用戶端的資料。
    * **要求**： 的 HTTP 要求計數。
    * **平均回應時間**： 之 web 應用程式，以回應 HTTP 要求的平均時間。

    此頁面上，也會找到自助服務的數個工具，適用於疑難排解和最佳化。

    ![自助服務工具](./media/monitoring/wizards.png)

    * **診斷並解決問題**是自助疑難排解。
    * **Application Insights**適用於分析效能和應用程式的行為，並稍後再討論這一節。
    * **App Service Advisor**提出來微調您的應用程式體驗的建議。

## <a name="advanced-monitoring"></a>進階監視

[Azure 監視器](/azure/monitoring-and-diagnostics/)來監視所有的度量和設定警示的 Azure 服務的集中式服務。 Azure 監視器中的系統管理員可以細微的方式追蹤效能，並找出趨勢。 每個 Azure 服務提供它自己[組計量](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions)Azure 監視器。

## <a name="profile-with-application-insights"></a>使用 Application Insights 設定檔

[Application Insights](/azure/application-insights/app-insights-overview)是一項 Azure 服務來分析效能和穩定性的 web 應用程式和使用者如何使用它們。 將資料從 Application Insights 是更廣泛、 更深入的 Azure 監視器。 資料可以提供開發人員和系統管理員，以改善應用程式的金鑰資訊。 Application Insights 可以新增至 Azure App Service 資源，無須變更程式碼中。

1. 開啟[Azure 入口網站](https://portal.azure.com)，然後瀏覽至*mywebapp\<unique_number\>*  App Service。
1. 從**概觀**索引標籤上，按一下**Application Insights**圖格。

    ![Application Insights 磚](./media/monitoring/app-insights.png)

1. 選取 **建立新的資源**選項按鈕。 使用預設的資源名稱，然後選取 Application Insights 資源的位置。 位置不需要符合您的 web 應用程式。

    ![Application Insights 安裝程式](./media/monitoring/new-app-insights.png)

1. 針對**執行階段/架構**，選取**ASP.NET Core**。 接受預設設定。
1. 選取 [確定]。 如果系統提示您確認時，選取**繼續**。
1. 在建立資源之後，請按一下 Application Insights 資源，直接瀏覽到 Application Insights 頁面的名稱。

    ![新的 Application Insights 資源已準備就緒](./media/monitoring/new-app-insights-done.png)

使用應用程式時，資料會累積。 選取 **重新整理**重新載入新的資料在刀鋒視窗。

![Application Insights [概觀] 索引標籤](./media/monitoring/app-insights-overview.png)

Application Insights 會提供有用的伺服器端資訊，不需要額外的設定。 若要從 Application Insights 取得最大價值[檢測您的應用程式使用 Application Insights SDK](/azure/application-insights/app-insights-asp-net-core)。 正確設定時，服務會提供端對端監視所有 web 伺服器和瀏覽器，包括用戶端效能。 如需詳細資訊，請參閱 < [Application Insights 文件](/azure/application-insights/app-insights-overview)。

## <a name="logging"></a>記錄

在 Azure App Service 中的預設會停用 web 伺服器與應用程式記錄檔。 啟用記錄檔進行下列步驟：

1. 開啟[Azure 入口網站](https://portal.azure.com)，然後瀏覽至*mywebapp\<unique_number\>*  App Service。
1. 在左側功能表中，向下捲動至**監視**一節。 選取 **診斷記錄**。

    ![診斷記錄 連結](./media/monitoring/logging.png)

1. 開啟**應用程式記錄 （檔案系統）**。 如果出現提示，請按一下此方塊可安裝的擴充功能，讓登入 web 應用程式的應用程式。
1. 設定**Web 伺服器記錄**要**檔案系統**。
1. 請輸入**保留期限**以天為單位。 例如，30。
1. 按一下 [儲存] 。

ASP.NET Core 與 web 伺服器 (App Service) 記錄檔會產生 web 應用程式。 他們可以使用顯示的 FTP/FTPS 資訊進行下載。 在本指南稍早建立的部署認證相同的密碼。 記錄檔可以是[串流處理到您使用 PowerShell 或 Azure CLI 的本機電腦直接](/azure/app-service/web-sites-enable-diagnostic-log#download)。 記錄檔也可以[Application Insights 中檢視](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights)。

## <a name="log-streaming"></a>記錄資料流

透過入口網站的即時串流處理應用程式和 web 伺服器記錄檔。

1. 開啟[Azure 入口網站](https://portal.azure.com)，然後瀏覽至*mywebapp\<unique_number\>*  App Service。
1. 在左側功能表中，向下捲動至**監視**區段，然後選取**記錄資料流**。

    ![記錄資料流連結](./media/monitoring/log-stream.png)

記錄檔也可以[透過 Azure CLI 或 Azure PowerShell 串流](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs)，包括透過 Cloud Shell。

## <a name="alerts"></a>警示

Azure 監視器 」 也提供[即時警示](/azure/monitoring-and-diagnostics/insights-alerts-portal)根據計量、 系統管理事件及其他準則。

> *注意： 目前 web 應用程式計量的警示僅供以警示 （傳統） 服務。*

[警示 （傳統） 服務](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal)您可以在 Azure 監視器中或之下找到**監視**的 App Service 設定 區段。

![警示 （傳統） 連結](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>即時偵錯

可以是 azure App Service[使用 Visual Studio 的遠端偵錯](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)當記錄檔未提供足夠的資訊。 不過，遠端偵錯需要應用程式使用偵錯符號進行編譯。 偵錯不應該進行生產環境中，除了最後的手段。

## <a name="conclusion"></a>結論

在本節中，您必須完成下列工作：

* 尋找基本的監視和疑難排解在 Azure 入口網站中的資料
* 了解 Azure 監視器的所有 Azure 服務所提供的更深入的了解計量
* 使用 Application Insights 時使用的 web 應用程式連線的應用程式程式碼剖析
* 開啟記錄功能，並了解如何下載記錄檔
* Stream 即時記錄檔
* 了解如何設定警示
* 深入了解遠端偵錯 Azure App Service web apps。

## <a name="additional-reading"></a>其他閱讀資料

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [監視 Azure web 應用程式效能，使用 Application Insights](/azure/application-insights/app-insights-azure-web-apps)
* [為 Azure App Service 中的 Web 應用程式啟用診斷記錄](/azure/app-service/web-sites-enable-diagnostic-log)
* [使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Azure 監視器中建立傳統計量警示，Azure 服務-Azure 入口網站](/azure/monitoring-and-diagnostics/insights-alerts-portal)
