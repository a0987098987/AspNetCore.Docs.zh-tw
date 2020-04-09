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
# <a name="monitor-and-debug"></a><span data-ttu-id="fde47-103">監視並除錯</span><span class="sxs-lookup"><span data-stu-id="fde47-103">Monitor and debug</span></span>

<span data-ttu-id="fde47-104">部署了應用並構建了 DevOps 管道後,瞭解如何監視和排除應用故障非常重要。</span><span class="sxs-lookup"><span data-stu-id="fde47-104">Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.</span></span>

<span data-ttu-id="fde47-105">在本節中,您將完成以下任務:</span><span class="sxs-lookup"><span data-stu-id="fde47-105">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="fde47-106">在 Azure 門戶中尋找基本監視和故障排除資料</span><span class="sxs-lookup"><span data-stu-id="fde47-106">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="fde47-107">瞭解 Azure 監視器如何深入瞭解所有 Azure 服務的指標</span><span class="sxs-lookup"><span data-stu-id="fde47-107">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="fde47-108">將 Web 應用程式與應用程式連接器,進行應用分析</span><span class="sxs-lookup"><span data-stu-id="fde47-108">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="fde47-109">開啟紀錄紀錄並瞭解下載紀錄的位置</span><span class="sxs-lookup"><span data-stu-id="fde47-109">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="fde47-110">即時串流記錄</span><span class="sxs-lookup"><span data-stu-id="fde47-110">Stream logs in real time</span></span>
* <span data-ttu-id="fde47-111">瞭解在何處設置警報</span><span class="sxs-lookup"><span data-stu-id="fde47-111">Learn where to set up alerts</span></span>
* <span data-ttu-id="fde47-112">瞭解遠端除錯 Azure 應用服務 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="fde47-112">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="basic-monitoring-and-troubleshooting"></a><span data-ttu-id="fde47-113">基本監控和故障排除</span><span class="sxs-lookup"><span data-stu-id="fde47-113">Basic monitoring and troubleshooting</span></span>

<span data-ttu-id="fde47-114">應用服務 Web 應用易於即時監控。</span><span class="sxs-lookup"><span data-stu-id="fde47-114">App Service web apps are easily monitored in real time.</span></span> <span data-ttu-id="fde47-115">Azure 門戶以易於理解的圖表和圖形呈現指標。</span><span class="sxs-lookup"><span data-stu-id="fde47-115">The Azure portal renders metrics in easy-to-understand charts and graphs.</span></span>

1. <span data-ttu-id="fde47-116">打開[Azure 門戶](https://portal.azure.com),然後導航到*\<\>mywebapp unique_number*應用服務。</span><span class="sxs-lookup"><span data-stu-id="fde47-116">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>

1. <span data-ttu-id="fde47-117">**"概述**"選項卡顯示有用的"概覽"資訊,包括顯示最近指標的圖形。</span><span class="sxs-lookup"><span data-stu-id="fde47-117">The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.</span></span>

    ![顯示概述面板的螢幕截圖](./media/monitoring/overview.png)

    * <span data-ttu-id="fde47-119">**HTTP 5xx**: 伺服器端錯誤計數,通常是ASP.NET核心代碼中的異常。</span><span class="sxs-lookup"><span data-stu-id="fde47-119">**Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.</span></span>
    * <span data-ttu-id="fde47-120">**資料輸入**: 資料入口進入您的 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="fde47-120">**Data In**: Data ingress coming into your web app.</span></span>
    * <span data-ttu-id="fde47-121">**數據出**:從 Web 應用向客戶端的數據出口。</span><span class="sxs-lookup"><span data-stu-id="fde47-121">**Data Out**: Data egress from your web app to clients.</span></span>
    * <span data-ttu-id="fde47-122">**請求**:HTTP 請求的計數。</span><span class="sxs-lookup"><span data-stu-id="fde47-122">**Requests**: Count of HTTP requests.</span></span>
    * <span data-ttu-id="fde47-123">**平均回應時間**:Web 應用回應 HTTP 請求的平均時間。</span><span class="sxs-lookup"><span data-stu-id="fde47-123">**Average Response Time**: Average time for the web app to respond to HTTP requests.</span></span>

    <span data-ttu-id="fde47-124">此頁面上還提供了幾個用於故障排除和優化的自助服務工具。</span><span class="sxs-lookup"><span data-stu-id="fde47-124">Several self-service tools for troubleshooting and optimization are also found on this page.</span></span>

    ![顯示自助服務工具的螢幕截圖](./media/monitoring/wizards.png)

    * <span data-ttu-id="fde47-126">**診斷和解決問題**是一個自助服務疑難解答。</span><span class="sxs-lookup"><span data-stu-id="fde47-126">**Diagnose and solve problems** is a self-service troubleshooter.</span></span>
    * <span data-ttu-id="fde47-127">**應用程式見解**用於分析性能和應用行為,本節稍後將對此進行討論。</span><span class="sxs-lookup"><span data-stu-id="fde47-127">**Application Insights** is for profiling performance and app behavior, and is discussed later in this section.</span></span>
    * <span data-ttu-id="fde47-128">**應用服務顧問**提供建議來調整你的應用體驗。</span><span class="sxs-lookup"><span data-stu-id="fde47-128">**App Service Advisor** makes recommendations to tune your app experience.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="fde47-129">進階監視</span><span class="sxs-lookup"><span data-stu-id="fde47-129">Advanced monitoring</span></span>

<span data-ttu-id="fde47-130">[Azure 監視器](/azure/monitoring-and-diagnostics/)是監視所有指標並跨 Azure 服務設置警報的集中服務。</span><span class="sxs-lookup"><span data-stu-id="fde47-130">[Azure Monitor](/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services.</span></span> <span data-ttu-id="fde47-131">在 Azure 監視器中,管理員可以精細跟蹤性能並識別趨勢。</span><span class="sxs-lookup"><span data-stu-id="fde47-131">Within Azure Monitor, administrators can granularly track performance and identify trends.</span></span> <span data-ttu-id="fde47-132">每個 Azure 服務向 Azure 監視器提供自己的[指標集](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions)。</span><span class="sxs-lookup"><span data-stu-id="fde47-132">Each Azure service offers its own [set of metrics](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.</span></span>

## <a name="profile-with-application-insights"></a><span data-ttu-id="fde47-133">具有應用程式見解的設定檔</span><span class="sxs-lookup"><span data-stu-id="fde47-133">Profile with Application Insights</span></span>

<span data-ttu-id="fde47-134">[應用程式見解](/azure/application-insights/app-insights-overview)是一種 Azure 服務,用於分析 Web 應用的性能和穩定性以及使用者如何使用它們。</span><span class="sxs-lookup"><span data-stu-id="fde47-134">[Application Insights](/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them.</span></span> <span data-ttu-id="fde47-135">來自應用程式見解的數據比 Azure 監視器的數據更廣泛和更深。</span><span class="sxs-lookup"><span data-stu-id="fde47-135">The data from Application Insights is broader and deeper than that of Azure Monitor.</span></span> <span data-ttu-id="fde47-136">這些數據可以為開發人員和管理員提供用於改進應用的關鍵資訊。</span><span class="sxs-lookup"><span data-stu-id="fde47-136">The data can provide developers and administrators with key information for improving apps.</span></span> <span data-ttu-id="fde47-137">應用程式見解可以添加到 Azure 應用服務資源,而無需更改代碼。</span><span class="sxs-lookup"><span data-stu-id="fde47-137">Application Insights can be added to an Azure App Service resource without code changes.</span></span>

1. <span data-ttu-id="fde47-138">打開[Azure 門戶](https://portal.azure.com),然後導航到*\<\>mywebapp unique_number*應用服務。</span><span class="sxs-lookup"><span data-stu-id="fde47-138">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="fde47-139">在 **「概述」** 選項卡中,按一下「**應用程式見解」** 磁貼。</span><span class="sxs-lookup"><span data-stu-id="fde47-139">From the **Overview** tab, click the **Application Insights** tile.</span></span>

    ![Application Insights 圖格](./media/monitoring/app-insights.png)

1. <span data-ttu-id="fde47-141">選擇「**建立新資源**單選」 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fde47-141">Select the **Create new resource** radio button.</span></span> <span data-ttu-id="fde47-142">使用預設資源名稱,然後選擇應用程式見解資源的位置。</span><span class="sxs-lookup"><span data-stu-id="fde47-142">Use the default resource name, and select the location for the Application Insights resource.</span></span> <span data-ttu-id="fde47-143">該位置不需要與 Web 應用的位置匹配。</span><span class="sxs-lookup"><span data-stu-id="fde47-143">The location doesn't need to match that of your web app.</span></span>

    ![應用程式見解設定](./media/monitoring/new-app-insights.png)

1. <span data-ttu-id="fde47-145">在**執行時/框架**,選擇**ASP.NET 核心**。</span><span class="sxs-lookup"><span data-stu-id="fde47-145">For **Runtime/Framework**, select **ASP.NET Core**.</span></span> <span data-ttu-id="fde47-146">接受預設設置。</span><span class="sxs-lookup"><span data-stu-id="fde47-146">Accept the default settings.</span></span>
1. <span data-ttu-id="fde47-147">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="fde47-147">Select **OK**.</span></span> <span data-ttu-id="fde47-148">如果提示確認,請選擇"**繼續**"。</span><span class="sxs-lookup"><span data-stu-id="fde47-148">If prompted to confirm, select **Continue**.</span></span>
1. <span data-ttu-id="fde47-149">創建資源後,按一下「應用程式見解」資源的名稱可直接導航到「應用程式見解」頁。</span><span class="sxs-lookup"><span data-stu-id="fde47-149">After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.</span></span>

    ![新的應用程式見解資源已準備就緒](./media/monitoring/new-app-insights-done.png)

<span data-ttu-id="fde47-151">使用應用時,數據會累積。</span><span class="sxs-lookup"><span data-stu-id="fde47-151">As the app is used, data accumulates.</span></span> <span data-ttu-id="fde47-152">選擇 **「刷新」** 以使用新資料重新載入邊欄選項卡。</span><span class="sxs-lookup"><span data-stu-id="fde47-152">Select **Refresh** to reload the blade with new data.</span></span>

![應用程式見解概述選項卡](./media/monitoring/app-insights-overview.png)

<span data-ttu-id="fde47-154">應用程式見解提供有用的伺服器端資訊,無需其他配置。</span><span class="sxs-lookup"><span data-stu-id="fde47-154">Application Insights provides useful server-side information with no additional configuration.</span></span> <span data-ttu-id="fde47-155">要從應用程式的解中取得最大價值,[請使用應用程式的見解 SDK 來偵測您的應用程式](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="fde47-155">To get the most value from Application Insights, [instrument your app with the Application Insights SDK](/azure/application-insights/app-insights-asp-net-core).</span></span> <span data-ttu-id="fde47-156">配置正確後,該服務可在 Web 伺服器和瀏覽器中提供端到端監視,包括用戶端性能。</span><span class="sxs-lookup"><span data-stu-id="fde47-156">When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance.</span></span> <span data-ttu-id="fde47-157">有關詳細資訊,請參閱[應用程式見解文件](/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="fde47-157">For more information, see the [Application Insights documentation](/azure/application-insights/app-insights-overview).</span></span>

## <a name="logging"></a><span data-ttu-id="fde47-158">記錄</span><span class="sxs-lookup"><span data-stu-id="fde47-158">Logging</span></span>

<span data-ttu-id="fde47-159">默認情況下,在 Azure 應用服務中禁用 Web 伺服器和應用日誌。</span><span class="sxs-lookup"><span data-stu-id="fde47-159">Web server and app logs are disabled by default in Azure App Service.</span></span> <span data-ttu-id="fde47-160">以以下步驟啟用日誌:</span><span class="sxs-lookup"><span data-stu-id="fde47-160">Enable the logs with the following steps:</span></span>

1. <span data-ttu-id="fde47-161">打開[Azure 門戶](https://portal.azure.com),然後導航到*\<\> mywebapp unique_number*應用服務。</span><span class="sxs-lookup"><span data-stu-id="fde47-161">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="fde47-162">在左側的功能表中,向下滾動到 **「監視」** 部分。</span><span class="sxs-lookup"><span data-stu-id="fde47-162">In the menu to the left, scroll down to the **Monitoring** section.</span></span> <span data-ttu-id="fde47-163">選擇**診斷紀錄**。</span><span class="sxs-lookup"><span data-stu-id="fde47-163">Select **Diagnostics logs**.</span></span>

    ![診斷紀錄連結](./media/monitoring/logging.png)

1. <span data-ttu-id="fde47-165">開啟**應用程式記錄(檔案系統)。**</span><span class="sxs-lookup"><span data-stu-id="fde47-165">Turn on **Application Logging (Filesystem)**.</span></span> <span data-ttu-id="fde47-166">如果出現提示,請按一個方塊以安裝擴展以在 Web 應用中啟用應用日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="fde47-166">If prompted, click the box to install the extensions to enable app logging in the web app.</span></span>
1. <span data-ttu-id="fde47-167">將**Web 伺服器紀錄紀錄**設定為**檔案系統**。</span><span class="sxs-lookup"><span data-stu-id="fde47-167">Set **Web server logging** to **File System**.</span></span>
1. <span data-ttu-id="fde47-168">輸入**保留期**(以天表示)。</span><span class="sxs-lookup"><span data-stu-id="fde47-168">Enter the **Retention Period** in days.</span></span> <span data-ttu-id="fde47-169">例如,30。</span><span class="sxs-lookup"><span data-stu-id="fde47-169">For example, 30.</span></span>
1. <span data-ttu-id="fde47-170">按一下 [檔案]  。</span><span class="sxs-lookup"><span data-stu-id="fde47-170">Click **Save**.</span></span>

<span data-ttu-id="fde47-171">ASP.NET為 Web 應用生成核心和 Web 伺服器(應用服務)日誌。</span><span class="sxs-lookup"><span data-stu-id="fde47-171">ASP.NET Core and web server (App Service) logs are generated for the web app.</span></span> <span data-ttu-id="fde47-172">可以使用顯示的 FTP/FTPS 資訊下載它們。</span><span class="sxs-lookup"><span data-stu-id="fde47-172">They can be downloaded using the FTP/FTPS information displayed.</span></span> <span data-ttu-id="fde47-173">密碼與本指南前面創建的部署憑據相同。</span><span class="sxs-lookup"><span data-stu-id="fde47-173">The password is the same as the deployment credentials created earlier in this guide.</span></span> <span data-ttu-id="fde47-174">紀錄可以使用[PowerShell 或 Azure CLI 直接流式傳輸到本地電腦](/azure/app-service/web-sites-enable-diagnostic-log#download)。</span><span class="sxs-lookup"><span data-stu-id="fde47-174">The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#download).</span></span> <span data-ttu-id="fde47-175">紀錄讓[應用程式時顯示解中檢視](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights)。</span><span class="sxs-lookup"><span data-stu-id="fde47-175">Logs can also be [viewed in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span></span>

## <a name="log-streaming"></a><span data-ttu-id="fde47-176">記錄串流</span><span class="sxs-lookup"><span data-stu-id="fde47-176">Log streaming</span></span>

<span data-ttu-id="fde47-177">應用和 Web 伺服器日誌可以通過門戶即時流式傳輸。</span><span class="sxs-lookup"><span data-stu-id="fde47-177">App and web server logs can be streamed in real time through the portal.</span></span>

1. <span data-ttu-id="fde47-178">打開[Azure 門戶](https://portal.azure.com),然後導航到*\<\> mywebapp unique_number*應用服務。</span><span class="sxs-lookup"><span data-stu-id="fde47-178">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="fde47-179">在左側的功能表中,向下滾動到 **「監視」** 部分,然後選擇 **「日誌流**」。</span><span class="sxs-lookup"><span data-stu-id="fde47-179">In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.</span></span>

    ![顯示紀錄串流連結的螢幕擷取](./media/monitoring/log-stream.png)

<span data-ttu-id="fde47-181">日誌也可以通過[Azure CLI 或 Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs)進行流式傳輸,包括透過雲外殼進行流式傳輸。</span><span class="sxs-lookup"><span data-stu-id="fde47-181">Logs can also be [streamed via Azure CLI or Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.</span></span>

## <a name="alerts"></a><span data-ttu-id="fde47-182">警示</span><span class="sxs-lookup"><span data-stu-id="fde47-182">Alerts</span></span>

<span data-ttu-id="fde47-183">Azure 監視器還根據指標、管理事件和其他條件提供[即時警報](/azure/monitoring-and-diagnostics/insights-alerts-portal)。</span><span class="sxs-lookup"><span data-stu-id="fde47-183">Azure Monitor also provides [real time alerts](/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.</span></span>

> <span data-ttu-id="fde47-184">*注意:當前 Web 應用指標上的警報僅在警報(經典)服務中可用。*</span><span class="sxs-lookup"><span data-stu-id="fde47-184">*Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*</span></span>

<span data-ttu-id="fde47-185">[警報(經典)服務](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal)可在 Azure 監視器或應用服務設置的**監視**部分中找到。</span><span class="sxs-lookup"><span data-stu-id="fde47-185">The [Alerts (classic) service](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.</span></span>

![警示(經典)連結](./media/monitoring/alerts.png)

## <a name="live-debugging"></a><span data-ttu-id="fde47-187">即時除錯</span><span class="sxs-lookup"><span data-stu-id="fde47-187">Live debugging</span></span>

<span data-ttu-id="fde47-188">當日誌不能提供足夠的資訊時,Azure 應用服務可以通過[Visual Studio 遠端調試](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)。</span><span class="sxs-lookup"><span data-stu-id="fde47-188">Azure App Service can be [debugged remotely with Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information.</span></span> <span data-ttu-id="fde47-189">但是,遠端調試要求使用調試符號編譯應用。</span><span class="sxs-lookup"><span data-stu-id="fde47-189">However, remote debugging requires the app to be compiled with debug symbols.</span></span> <span data-ttu-id="fde47-190">調試不應在生產中完成,除非是最後的手段。</span><span class="sxs-lookup"><span data-stu-id="fde47-190">Debugging shouldn't be done in production, except as a last resort.</span></span>

## <a name="conclusion"></a><span data-ttu-id="fde47-191">結論</span><span class="sxs-lookup"><span data-stu-id="fde47-191">Conclusion</span></span>

<span data-ttu-id="fde47-192">在本節中,您完成了以下任務:</span><span class="sxs-lookup"><span data-stu-id="fde47-192">In this section, you completed the following tasks:</span></span>

* <span data-ttu-id="fde47-193">在 Azure 門戶中尋找基本監視和故障排除資料</span><span class="sxs-lookup"><span data-stu-id="fde47-193">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="fde47-194">瞭解 Azure 監視器如何深入瞭解所有 Azure 服務的指標</span><span class="sxs-lookup"><span data-stu-id="fde47-194">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="fde47-195">將 Web 應用程式與應用程式連接器,進行應用分析</span><span class="sxs-lookup"><span data-stu-id="fde47-195">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="fde47-196">開啟紀錄紀錄並瞭解下載紀錄的位置</span><span class="sxs-lookup"><span data-stu-id="fde47-196">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="fde47-197">即時串流記錄</span><span class="sxs-lookup"><span data-stu-id="fde47-197">Stream logs in real time</span></span>
* <span data-ttu-id="fde47-198">瞭解在何處設置警報</span><span class="sxs-lookup"><span data-stu-id="fde47-198">Learn where to set up alerts</span></span>
* <span data-ttu-id="fde47-199">瞭解遠端除錯 Azure 應用服務 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="fde47-199">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="fde47-200">其他閱讀資料</span><span class="sxs-lookup"><span data-stu-id="fde47-200">Additional reading</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="fde47-201">使用 Application Insights 監視 Azure Web 應用程式效能</span><span class="sxs-lookup"><span data-stu-id="fde47-201">Monitor Azure web app performance with Application Insights</span></span>](/azure/application-insights/app-insights-azure-web-apps)
* [<span data-ttu-id="fde47-202">在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能。</span><span class="sxs-lookup"><span data-stu-id="fde47-202">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)
* [<span data-ttu-id="fde47-203">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="fde47-203">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="fde47-204">在 Azure 監視器中為 Azure 服務建立傳統計量警示 - Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="fde47-204">Create classic metric alerts in Azure Monitor for Azure services - Azure portal</span></span>](/azure/monitoring-and-diagnostics/insights-alerts-portal)
