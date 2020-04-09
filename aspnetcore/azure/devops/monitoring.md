---
title: 監視並除錯 - 使用 ASP.NET 核心與 Azure 進行 DevOps
author: CamSoper
description: 使用ASP.NET核心和 Azure 監視和除錯程式作為 DevOps 解決方案的一部分
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 07/10/2019
uid: azure/devops/monitor
ms.openlocfilehash: 1d8ed99f4387dbc99929164c558cc2ce14bd9ea0
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78659499"
---
# <a name="monitor-and-debug"></a>監視並除錯

部署了應用並構建了 DevOps 管道後,瞭解如何監視和排除應用故障非常重要。

在本節中,您將完成以下任務:

* 在 Azure 門戶中尋找基本監視和故障排除資料
* 瞭解 Azure 監視器如何深入瞭解所有 Azure 服務的指標
* 將 Web 應用程式與應用程式連接器,進行應用分析
* 開啟紀錄紀錄並瞭解下載紀錄的位置
* 即時串流記錄
* 瞭解在何處設置警報
* 瞭解遠端除錯 Azure 應用服務 Web 應用。

## <a name="basic-monitoring-and-troubleshooting"></a>基本監控和故障排除

應用服務 Web 應用易於即時監控。 Azure 門戶以易於理解的圖表和圖形呈現指標。

1. 打開[Azure 門戶](https://portal.azure.com),然後導航到*\<\>mywebapp unique_number*應用服務。

1. **"概述**"選項卡顯示有用的"概覽"資訊,包括顯示最近指標的圖形。

    ![顯示概述面板的螢幕截圖](./media/monitoring/overview.png)

    * **HTTP 5xx**: 伺服器端錯誤計數,通常是ASP.NET核心代碼中的異常。
    * **資料輸入**: 資料入口進入您的 Web 應用。
    * **數據出**:從 Web 應用向客戶端的數據出口。
    * **請求**:HTTP 請求的計數。
    * **平均回應時間**:Web 應用回應 HTTP 請求的平均時間。

    此頁面上還提供了幾個用於故障排除和優化的自助服務工具。

    ![顯示自助服務工具的螢幕截圖](./media/monitoring/wizards.png)

    * **診斷和解決問題**是一個自助服務疑難解答。
    * **應用程式見解**用於分析性能和應用行為,本節稍後將對此進行討論。
    * **應用服務顧問**提供建議來調整你的應用體驗。

## <a name="advanced-monitoring"></a>進階監視

[Azure 監視器](/azure/monitoring-and-diagnostics/)是監視所有指標並跨 Azure 服務設置警報的集中服務。 在 Azure 監視器中,管理員可以精細跟蹤性能並識別趨勢。 每個 Azure 服務向 Azure 監視器提供自己的[指標集](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions)。

## <a name="profile-with-application-insights"></a>具有應用程式見解的設定檔

[應用程式見解](/azure/application-insights/app-insights-overview)是一種 Azure 服務,用於分析 Web 應用的性能和穩定性以及使用者如何使用它們。 來自應用程式見解的數據比 Azure 監視器的數據更廣泛和更深。 這些數據可以為開發人員和管理員提供用於改進應用的關鍵資訊。 應用程式見解可以添加到 Azure 應用服務資源,而無需更改代碼。

1. 打開[Azure 門戶](https://portal.azure.com),然後導航到*\<\>mywebapp unique_number*應用服務。
1. 在 **「概述」** 選項卡中,按一下「**應用程式見解」** 磁貼。

    ![Application Insights 圖格](./media/monitoring/app-insights.png)

1. 選擇「**建立新資源**單選」 按鈕。 使用預設資源名稱,然後選擇應用程式見解資源的位置。 該位置不需要與 Web 應用的位置匹配。

    ![應用程式見解設定](./media/monitoring/new-app-insights.png)

1. 在**執行時/框架**,選擇**ASP.NET 核心**。 接受預設設置。
1. 選取 [確定]  。 如果提示確認,請選擇"**繼續**"。
1. 創建資源後,按一下「應用程式見解」資源的名稱可直接導航到「應用程式見解」頁。

    ![新的應用程式見解資源已準備就緒](./media/monitoring/new-app-insights-done.png)

使用應用時,數據會累積。 選擇 **「刷新」** 以使用新資料重新載入邊欄選項卡。

![應用程式見解概述選項卡](./media/monitoring/app-insights-overview.png)

應用程式見解提供有用的伺服器端資訊,無需其他配置。 要從應用程式的解中取得最大價值,[請使用應用程式的見解 SDK 來偵測您的應用程式](/azure/application-insights/app-insights-asp-net-core)。 配置正確後,該服務可在 Web 伺服器和瀏覽器中提供端到端監視,包括用戶端性能。 有關詳細資訊,請參閱[應用程式見解文件](/azure/application-insights/app-insights-overview)。

## <a name="logging"></a>記錄

默認情況下,在 Azure 應用服務中禁用 Web 伺服器和應用日誌。 以以下步驟啟用日誌:

1. 打開[Azure 門戶](https://portal.azure.com),然後導航到*\<\> mywebapp unique_number*應用服務。
1. 在左側的功能表中,向下滾動到 **「監視」** 部分。 選擇**診斷紀錄**。

    ![診斷紀錄連結](./media/monitoring/logging.png)

1. 開啟**應用程式記錄(檔案系統)。** 如果出現提示,請按一個方塊以安裝擴展以在 Web 應用中啟用應用日誌記錄。
1. 將**Web 伺服器紀錄紀錄**設定為**檔案系統**。
1. 輸入**保留期**(以天表示)。 例如,30。
1. 按一下 [檔案]  。

ASP.NET為 Web 應用生成核心和 Web 伺服器(應用服務)日誌。 可以使用顯示的 FTP/FTPS 資訊下載它們。 密碼與本指南前面創建的部署憑據相同。 紀錄可以使用[PowerShell 或 Azure CLI 直接流式傳輸到本地電腦](/azure/app-service/web-sites-enable-diagnostic-log#download)。 紀錄讓[應用程式時顯示解中檢視](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights)。

## <a name="log-streaming"></a>記錄串流

應用和 Web 伺服器日誌可以通過門戶即時流式傳輸。

1. 打開[Azure 門戶](https://portal.azure.com),然後導航到*\<\> mywebapp unique_number*應用服務。
1. 在左側的功能表中,向下滾動到 **「監視」** 部分,然後選擇 **「日誌流**」。

    ![顯示紀錄串流連結的螢幕擷取](./media/monitoring/log-stream.png)

日誌也可以通過[Azure CLI 或 Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs)進行流式傳輸,包括透過雲外殼進行流式傳輸。

## <a name="alerts"></a>警示

Azure 監視器還根據指標、管理事件和其他條件提供[即時警報](/azure/monitoring-and-diagnostics/insights-alerts-portal)。

> *注意:當前 Web 應用指標上的警報僅在警報(經典)服務中可用。*

[警報(經典)服務](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal)可在 Azure 監視器或應用服務設置的**監視**部分中找到。

![警示(經典)連結](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>即時除錯

當日誌不能提供足夠的資訊時,Azure 應用服務可以通過[Visual Studio 遠端調試](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)。 但是,遠端調試要求使用調試符號編譯應用。 調試不應在生產中完成,除非是最後的手段。

## <a name="conclusion"></a>結論

在本節中,您完成了以下任務:

* 在 Azure 門戶中尋找基本監視和故障排除資料
* 瞭解 Azure 監視器如何深入瞭解所有 Azure 服務的指標
* 將 Web 應用程式與應用程式連接器,進行應用分析
* 開啟紀錄紀錄並瞭解下載紀錄的位置
* 即時串流記錄
* 瞭解在何處設置警報
* 瞭解遠端除錯 Azure 應用服務 Web 應用。

## <a name="additional-reading"></a>其他閱讀資料

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [使用 Application Insights 監視 Azure Web 應用程式效能](/azure/application-insights/app-insights-azure-web-apps)
* [在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能。](/azure/app-service/web-sites-enable-diagnostic-log)
* [使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [在 Azure 監視器中為 Azure 服務建立傳統計量警示 - Azure 入口網站](/azure/monitoring-and-diagnostics/insights-alerts-portal)
