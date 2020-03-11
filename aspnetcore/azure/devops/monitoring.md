---
title: 使用 ASP.NET Core 和 Azure 進行監視和 DevOps
author: CamSoper
description: 使用 ASP.NET Core 和 Azure，以 DevOps 解決方案的一部分監視和偵錯工具代碼
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 07/10/2019
uid: azure/devops/monitor
ms.openlocfilehash: 1d8ed99f4387dbc99929164c558cc2ce14bd9ea0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659499"
---
# <a name="monitor-and-debug"></a>監視和調試

部署應用程式並建立 DevOps 管線之後，請務必瞭解如何監視應用程式並對其進行疑難排解。

在本節中，您將完成下列工作：

* 尋找 Azure 入口網站中的基本監視和疑難排解資料
* 瞭解 Azure 監視器如何讓您深入瞭解所有 Azure 服務的計量
* 將 web 應用程式與用於應用程式分析的 Application Insights 進行連線
* 開啟記錄功能，並瞭解下載記錄檔的位置
* 即時串流記錄
* 瞭解設定警示的位置
* 深入瞭解 Azure App Service web 應用程式的遠端偵錯程式。

## <a name="basic-monitoring-and-troubleshooting"></a>基本監視和疑難排解

App Service 的 web 應用程式會即時受到輕鬆監視。 Azure 入口網站會以易於瞭解的圖表和圖形呈現計量。

1. 開啟 [ [Azure 入口網站](https://portal.azure.com)]，然後流覽至 [ *mywebapp\<unique_number\>* App Service]。

1. [**總覽**] 索引標籤會顯示有用的「一覽」資訊，包括顯示最近度量的圖形。

    ![顯示總覽面板的螢幕擷取畫面](./media/monitoring/overview.png)

    * **Http 5xx**：伺服器端錯誤的計數，通常是 ASP.NET Core 程式碼中的例外狀況。
    * **中的資料**：資料輸入進入您的 web 應用程式。
    * **資料輸出**：從 web 應用程式到用戶端的資料輸出。
    * **要求**： HTTP 要求的計數。
    * **平均回應時間**： web 應用程式回應 HTTP 要求的平均時間。

    您也可以在此頁面上找到數個用於疑難排解和優化的自助工具。

    ![顯示自助服務工具的螢幕擷取畫面](./media/monitoring/wizards.png)

    * **診斷並解決問題**是自助服務的疑難排解員。
    * **Application Insights**適用于分析效能和應用程式行為，稍後將在本節中討論。
    * **App Service Advisor**提出建議來調整您的應用程式體驗。

## <a name="advanced-monitoring"></a>進階監視

[Azure 監視器](/azure/monitoring-and-diagnostics/)是集中式服務，可跨 Azure 服務監視所有計量和設定警示。 在 Azure 監視器內，系統管理員可以精細地追蹤效能並找出趨勢。 每個 Azure 服務都提供自己[的一組計量](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions)來 Azure 監視器。

## <a name="profile-with-application-insights"></a>使用 Application Insights 的設定檔

[Application Insights](/azure/application-insights/app-insights-overview)是一項 Azure 服務，可用於分析 web 應用程式的效能和穩定性，以及使用者使用它們的方式。 Application Insights 的資料比 Azure 監視器更廣且更深入。 資料可為開發人員和系統管理員提供改善應用程式的重要資訊。 Application Insights 可以新增至 Azure App Service 資源，而不需要變更程式碼。

1. 開啟 [ [Azure 入口網站](https://portal.azure.com)]，然後流覽至 [ *mywebapp\<unique_number\>* App Service]。
1. 從 [**總覽**] 索引標籤中，按一下 [ **Application Insights** ] 磚。

    ![Application Insights 圖格](./media/monitoring/app-insights.png)

1. 選取 [**建立新資源**] 選項按鈕。 使用預設的資源名稱，然後選取 Application Insights 資源的位置。 位置不需要與您的 web 應用程式相符。

    ![Application Insights 設定](./media/monitoring/new-app-insights.png)

1. 針對 [**執行時間/架構**]，選取 [ **ASP.NET Core**]。 接受預設設定。
1. 選取 [確定]。 如果系統提示您確認，請選取 [**繼續**]。
1. 建立資源之後，請按一下 Application Insights 資源的名稱，直接流覽至 [Application Insights] 頁面。

    ![已準備好新的 Application Insights 資源](./media/monitoring/new-app-insights-done.png)

使用應用程式時，資料會累積。 選取 **[** 重新整理]，以使用新的資料重載此分頁。

![Application Insights 總覽 索引標籤](./media/monitoring/app-insights-overview.png)

Application Insights 提供有用的伺服器端資訊，而不需進行其他設定。 若要充分發揮 Application Insights 的價值，請[使用 APPLICATION INSIGHTS SDK 檢測您的應用程式](/azure/application-insights/app-insights-asp-net-core)。 當設定正確時，服務會跨網頁伺服器和瀏覽器提供端對端監視，包括用戶端效能。 如需詳細資訊，請參閱[Application Insights 檔](/azure/application-insights/app-insights-overview)。

## <a name="logging"></a>記錄

在 Azure App Service 中，預設會停用 Web 服務器和應用程式記錄檔。 使用下列步驟啟用記錄：

1. 開啟[Azure 入口網站](https://portal.azure.com)，然後流覽至*mywebapp\<unique_number\>* App Service。
1. 在左側功能表中，向下流覽至 [**監視**] 區段。 選取 [**診斷記錄**]。

    ![診斷記錄連結](./media/monitoring/logging.png)

1. 開啟 **[應用程式記錄（檔案系統）** ]。 若出現提示，請按一下方塊以安裝延伸模組，以在 web 應用程式中啟用應用程式記錄。
1. 將 [ **Web 服務器記錄**] 設定為 [**檔案系統**]。
1. 輸入**保留期限**（以天為單位）。 例如，30。
1. 按一下 [檔案]。

ASP.NET Core 和 web 伺服器（App Service）記錄會針對 web 應用程式產生。 您可以使用顯示的 FTP/FTPS 資訊來下載它們。 此密碼與本指南稍早建立的部署認證相同。 [您可以使用 PowerShell 或 Azure CLI，將記錄直接串流至您的本機電腦](/azure/app-service/web-sites-enable-diagnostic-log#download)。 您也可以[在 Application Insights 中查看](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights)記錄。

## <a name="log-streaming"></a>記錄串流

應用程式和 web 伺服器記錄可以透過入口網站即時串流。

1. 開啟[Azure 入口網站](https://portal.azure.com)，然後流覽至*mywebapp\<unique_number\>* App Service。
1. 在左側功能表中，向下流覽至 [**監視**] 區段，然後選取 [**記錄資料流程**]。

    ![顯示記錄資料流程連結的螢幕擷取畫面](./media/monitoring/log-stream.png)

記錄也可以透過[Azure CLI 或 Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs)進行串流處理，包括 Cloud Shell。

## <a name="alerts"></a>警示

Azure 監視器也會根據計量、系統管理事件和其他準則提供[即時警示](/azure/monitoring-and-diagnostics/insights-alerts-portal)。

> *注意：目前只有在 [警示（傳統）] 服務中才會提供 web 應用程式計量的警示。*

您可以在 Azure 監視器或 App Service 設定的 [**監視**] 區段下找到 [[警示（傳統）] 服務](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal)。

![警示（傳統）連結](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>即時調試

當記錄檔未提供足夠的資訊時[，可使用 Visual Studio 從遠端進行 Azure App Service 的調試](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)。 不過，遠端偵錯程式需要使用 debug 符號來編譯應用程式。 除了最後的手段以外，不應在生產環境中執行調試。

## <a name="conclusion"></a>結論

在本節中，您已完成下列工作：

* 尋找 Azure 入口網站中的基本監視和疑難排解資料
* 瞭解 Azure 監視器如何讓您深入瞭解所有 Azure 服務的計量
* 將 web 應用程式與用於應用程式分析的 Application Insights 進行連線
* 開啟記錄功能，並瞭解下載記錄檔的位置
* 即時串流記錄
* 瞭解設定警示的位置
* 深入瞭解 Azure App Service web 應用程式的遠端偵錯程式。

## <a name="additional-reading"></a>其他閱讀資料

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [使用 Application Insights 監視 Azure Web 應用程式效能](/azure/application-insights/app-insights-azure-web-apps)
* [在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能](/azure/app-service/web-sites-enable-diagnostic-log)
* [使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [在適用于 Azure 服務的 Azure 監視器中建立傳統計量警示-Azure 入口網站](/azure/monitoring-and-diagnostics/insights-alerts-portal)
