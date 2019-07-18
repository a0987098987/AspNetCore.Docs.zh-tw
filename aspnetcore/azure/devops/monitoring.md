---
title: 監視和偵錯-使用 ASP.NET Core 和 Azure 的 DevOps
author: CamSoper
description: 監視和 DevOps 解決方案偵錯您的程式碼，使用 ASP.NET Core 和 Azure
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 07/10/2019
uid: azure/devops/monitor
ms.openlocfilehash: 1d8ed99f4387dbc99929164c558cc2ce14bd9ea0
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2019
ms.locfileid: "68307951"
---
# <a name="monitor-and-debug"></a><span data-ttu-id="98bb6-103">監視和偵錯</span><span class="sxs-lookup"><span data-stu-id="98bb6-103">Monitor and debug</span></span>

<span data-ttu-id="98bb6-104">部署應用程式和內建的 DevOps 管線，請務必了解如何監視和疑難排解應用程式。</span><span class="sxs-lookup"><span data-stu-id="98bb6-104">Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.</span></span>

<span data-ttu-id="98bb6-105">在本節中，您將完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="98bb6-105">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="98bb6-106">尋找基本的監視和疑難排解在 Azure 入口網站中的資料</span><span class="sxs-lookup"><span data-stu-id="98bb6-106">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="98bb6-107">了解 Azure 監視器的所有 Azure 服務所提供的更深入的了解計量</span><span class="sxs-lookup"><span data-stu-id="98bb6-107">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="98bb6-108">使用 Application Insights 時使用的 web 應用程式連線的應用程式程式碼剖析</span><span class="sxs-lookup"><span data-stu-id="98bb6-108">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="98bb6-109">開啟記錄功能，並了解如何下載記錄檔</span><span class="sxs-lookup"><span data-stu-id="98bb6-109">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="98bb6-110">Stream 即時記錄檔</span><span class="sxs-lookup"><span data-stu-id="98bb6-110">Stream logs in real time</span></span>
* <span data-ttu-id="98bb6-111">了解如何設定警示</span><span class="sxs-lookup"><span data-stu-id="98bb6-111">Learn where to set up alerts</span></span>
* <span data-ttu-id="98bb6-112">深入了解遠端偵錯 Azure App Service web apps。</span><span class="sxs-lookup"><span data-stu-id="98bb6-112">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="basic-monitoring-and-troubleshooting"></a><span data-ttu-id="98bb6-113">基本監視和疑難排解</span><span class="sxs-lookup"><span data-stu-id="98bb6-113">Basic monitoring and troubleshooting</span></span>

<span data-ttu-id="98bb6-114">App Service web 應用程式輕鬆地即時監視。</span><span class="sxs-lookup"><span data-stu-id="98bb6-114">App Service web apps are easily monitored in real time.</span></span> <span data-ttu-id="98bb6-115">Azure 入口網站將呈現簡單易懂的圖表和圖形中的計量。</span><span class="sxs-lookup"><span data-stu-id="98bb6-115">The Azure portal renders metrics in easy-to-understand charts and graphs.</span></span>

1. <span data-ttu-id="98bb6-116">開啟[Azure 入口網站](https://portal.azure.com)，然後瀏覽至*mywebapp\<unique_number\>*  App Service。</span><span class="sxs-lookup"><span data-stu-id="98bb6-116">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>

1. <span data-ttu-id="98bb6-117">**概觀**索引標籤會顯示有用的 「 在摘要 」 資訊，包括顯示最近使用的計量的圖表。</span><span class="sxs-lookup"><span data-stu-id="98bb6-117">The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.</span></span>

    ![螢幕擷取畫面顯示概觀面板](./media/monitoring/overview.png)

    * <span data-ttu-id="98bb6-119">**Http 5xx**:伺服器端錯誤的計數, 通常是 ASP.NET Core 程式碼中的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="98bb6-119">**Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.</span></span>
    * <span data-ttu-id="98bb6-120">**中的資料**:資料輸入進入您的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="98bb6-120">**Data In**: Data ingress coming into your web app.</span></span>
    * <span data-ttu-id="98bb6-121">**資料輸出**:從您的 web 應用程式到用戶端的資料輸出。</span><span class="sxs-lookup"><span data-stu-id="98bb6-121">**Data Out**: Data egress from your web app to clients.</span></span>
    * <span data-ttu-id="98bb6-122">**要求**:HTTP 要求的計數。</span><span class="sxs-lookup"><span data-stu-id="98bb6-122">**Requests**: Count of HTTP requests.</span></span>
    * <span data-ttu-id="98bb6-123">**平均回應時間**:Web 應用程式回應 HTTP 要求的平均時間。</span><span class="sxs-lookup"><span data-stu-id="98bb6-123">**Average Response Time**: Average time for the web app to respond to HTTP requests.</span></span>

    <span data-ttu-id="98bb6-124">此頁面上，也會找到自助服務的數個工具，適用於疑難排解和最佳化。</span><span class="sxs-lookup"><span data-stu-id="98bb6-124">Several self-service tools for troubleshooting and optimization are also found on this page.</span></span>

    ![螢幕擷取畫面顯示自助工具](./media/monitoring/wizards.png)

    * <span data-ttu-id="98bb6-126">**診斷並解決問題**是自助疑難排解。</span><span class="sxs-lookup"><span data-stu-id="98bb6-126">**Diagnose and solve problems** is a self-service troubleshooter.</span></span>
    * <span data-ttu-id="98bb6-127">**Application Insights**適用於分析效能和應用程式的行為，並稍後再討論這一節。</span><span class="sxs-lookup"><span data-stu-id="98bb6-127">**Application Insights** is for profiling performance and app behavior, and is discussed later in this section.</span></span>
    * <span data-ttu-id="98bb6-128">**App Service Advisor**提出來微調您的應用程式體驗的建議。</span><span class="sxs-lookup"><span data-stu-id="98bb6-128">**App Service Advisor** makes recommendations to tune your app experience.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="98bb6-129">進階監視</span><span class="sxs-lookup"><span data-stu-id="98bb6-129">Advanced monitoring</span></span>

<span data-ttu-id="98bb6-130">[Azure 監視器](/azure/monitoring-and-diagnostics/)來監視所有的度量和設定警示的 Azure 服務的集中式服務。</span><span class="sxs-lookup"><span data-stu-id="98bb6-130">[Azure Monitor](/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services.</span></span> <span data-ttu-id="98bb6-131">Azure 監視器中的系統管理員可以細微的方式追蹤效能，並找出趨勢。</span><span class="sxs-lookup"><span data-stu-id="98bb6-131">Within Azure Monitor, administrators can granularly track performance and identify trends.</span></span> <span data-ttu-id="98bb6-132">每個 Azure 服務提供它自己[組計量](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions)Azure 監視器。</span><span class="sxs-lookup"><span data-stu-id="98bb6-132">Each Azure service offers its own [set of metrics](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.</span></span>

## <a name="profile-with-application-insights"></a><span data-ttu-id="98bb6-133">使用 Application Insights 設定檔</span><span class="sxs-lookup"><span data-stu-id="98bb6-133">Profile with Application Insights</span></span>

<span data-ttu-id="98bb6-134">[Application Insights](/azure/application-insights/app-insights-overview)是一項 Azure 服務來分析效能和穩定性的 web 應用程式和使用者如何使用它們。</span><span class="sxs-lookup"><span data-stu-id="98bb6-134">[Application Insights](/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them.</span></span> <span data-ttu-id="98bb6-135">將資料從 Application Insights 是更廣泛、 更深入的 Azure 監視器。</span><span class="sxs-lookup"><span data-stu-id="98bb6-135">The data from Application Insights is broader and deeper than that of Azure Monitor.</span></span> <span data-ttu-id="98bb6-136">資料可以提供開發人員和系統管理員，以改善應用程式的金鑰資訊。</span><span class="sxs-lookup"><span data-stu-id="98bb6-136">The data can provide developers and administrators with key information for improving apps.</span></span> <span data-ttu-id="98bb6-137">Application Insights 可以新增至 Azure App Service 資源，無須變更程式碼中。</span><span class="sxs-lookup"><span data-stu-id="98bb6-137">Application Insights can be added to an Azure App Service resource without code changes.</span></span>

1. <span data-ttu-id="98bb6-138">開啟[Azure 入口網站](https://portal.azure.com)，然後瀏覽至*mywebapp\<unique_number\>*  App Service。</span><span class="sxs-lookup"><span data-stu-id="98bb6-138">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="98bb6-139">從**概觀**索引標籤上，按一下**Application Insights**圖格。</span><span class="sxs-lookup"><span data-stu-id="98bb6-139">From the **Overview** tab, click the **Application Insights** tile.</span></span>

    ![Application Insights 磚](./media/monitoring/app-insights.png)

1. <span data-ttu-id="98bb6-141">選取 **建立新的資源**選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="98bb6-141">Select the **Create new resource** radio button.</span></span> <span data-ttu-id="98bb6-142">使用預設的資源名稱，然後選取 Application Insights 資源的位置。</span><span class="sxs-lookup"><span data-stu-id="98bb6-142">Use the default resource name, and select the location for the Application Insights resource.</span></span> <span data-ttu-id="98bb6-143">位置不需要符合您的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="98bb6-143">The location doesn't need to match that of your web app.</span></span>

    ![Application Insights 安裝程式](./media/monitoring/new-app-insights.png)

1. <span data-ttu-id="98bb6-145">針對**執行階段/架構**，選取**ASP.NET Core**。</span><span class="sxs-lookup"><span data-stu-id="98bb6-145">For **Runtime/Framework**, select **ASP.NET Core**.</span></span> <span data-ttu-id="98bb6-146">接受預設設定。</span><span class="sxs-lookup"><span data-stu-id="98bb6-146">Accept the default settings.</span></span>
1. <span data-ttu-id="98bb6-147">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="98bb6-147">Select **OK**.</span></span> <span data-ttu-id="98bb6-148">如果系統提示您確認時，選取**繼續**。</span><span class="sxs-lookup"><span data-stu-id="98bb6-148">If prompted to confirm, select **Continue**.</span></span>
1. <span data-ttu-id="98bb6-149">在建立資源之後，請按一下 Application Insights 資源，直接瀏覽到 Application Insights 頁面的名稱。</span><span class="sxs-lookup"><span data-stu-id="98bb6-149">After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.</span></span>

    ![新的 Application Insights 資源已準備就緒](./media/monitoring/new-app-insights-done.png)

<span data-ttu-id="98bb6-151">使用應用程式時，資料會累積。</span><span class="sxs-lookup"><span data-stu-id="98bb6-151">As the app is used, data accumulates.</span></span> <span data-ttu-id="98bb6-152">選取 **重新整理**重新載入新的資料在刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="98bb6-152">Select **Refresh** to reload the blade with new data.</span></span>

![Application Insights [概觀] 索引標籤](./media/monitoring/app-insights-overview.png)

<span data-ttu-id="98bb6-154">Application Insights 會提供有用的伺服器端資訊，不需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="98bb6-154">Application Insights provides useful server-side information with no additional configuration.</span></span> <span data-ttu-id="98bb6-155">若要從 Application Insights 取得最大價值[檢測您的應用程式使用 Application Insights SDK](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="98bb6-155">To get the most value from Application Insights, [instrument your app with the Application Insights SDK](/azure/application-insights/app-insights-asp-net-core).</span></span> <span data-ttu-id="98bb6-156">正確設定時，服務會提供端對端監視所有 web 伺服器和瀏覽器，包括用戶端效能。</span><span class="sxs-lookup"><span data-stu-id="98bb6-156">When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance.</span></span> <span data-ttu-id="98bb6-157">如需詳細資訊，請參閱 < [Application Insights 文件](/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="98bb6-157">For more information, see the [Application Insights documentation](/azure/application-insights/app-insights-overview).</span></span>

## <a name="logging"></a><span data-ttu-id="98bb6-158">記錄</span><span class="sxs-lookup"><span data-stu-id="98bb6-158">Logging</span></span>

<span data-ttu-id="98bb6-159">在 Azure App Service 中的預設會停用 web 伺服器與應用程式記錄檔。</span><span class="sxs-lookup"><span data-stu-id="98bb6-159">Web server and app logs are disabled by default in Azure App Service.</span></span> <span data-ttu-id="98bb6-160">啟用記錄檔進行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="98bb6-160">Enable the logs with the following steps:</span></span>

1. <span data-ttu-id="98bb6-161">開啟[Azure 入口網站](https://portal.azure.com)，然後瀏覽至*mywebapp\<unique_number\>*  App Service。</span><span class="sxs-lookup"><span data-stu-id="98bb6-161">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="98bb6-162">在左側功能表中，向下捲動至**監視**一節。</span><span class="sxs-lookup"><span data-stu-id="98bb6-162">In the menu to the left, scroll down to the **Monitoring** section.</span></span> <span data-ttu-id="98bb6-163">選取 **診斷記錄**。</span><span class="sxs-lookup"><span data-stu-id="98bb6-163">Select **Diagnostics logs**.</span></span>

    ![診斷記錄 連結](./media/monitoring/logging.png)

1. <span data-ttu-id="98bb6-165">開啟**應用程式記錄 （檔案系統）** 。</span><span class="sxs-lookup"><span data-stu-id="98bb6-165">Turn on **Application Logging (Filesystem)**.</span></span> <span data-ttu-id="98bb6-166">如果出現提示，請按一下此方塊可安裝的擴充功能，讓登入 web 應用程式的應用程式。</span><span class="sxs-lookup"><span data-stu-id="98bb6-166">If prompted, click the box to install the extensions to enable app logging in the web app.</span></span>
1. <span data-ttu-id="98bb6-167">設定**Web 伺服器記錄**要**檔案系統**。</span><span class="sxs-lookup"><span data-stu-id="98bb6-167">Set **Web server logging** to **File System**.</span></span>
1. <span data-ttu-id="98bb6-168">請輸入**保留期限**以天為單位。</span><span class="sxs-lookup"><span data-stu-id="98bb6-168">Enter the **Retention Period** in days.</span></span> <span data-ttu-id="98bb6-169">例如，30。</span><span class="sxs-lookup"><span data-stu-id="98bb6-169">For example, 30.</span></span>
1. <span data-ttu-id="98bb6-170">按一下 [儲存]  。</span><span class="sxs-lookup"><span data-stu-id="98bb6-170">Click **Save**.</span></span>

<span data-ttu-id="98bb6-171">ASP.NET Core 與 web 伺服器 (App Service) 記錄檔會產生 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="98bb6-171">ASP.NET Core and web server (App Service) logs are generated for the web app.</span></span> <span data-ttu-id="98bb6-172">他們可以使用顯示的 FTP/FTPS 資訊進行下載。</span><span class="sxs-lookup"><span data-stu-id="98bb6-172">They can be downloaded using the FTP/FTPS information displayed.</span></span> <span data-ttu-id="98bb6-173">在本指南稍早建立的部署認證相同的密碼。</span><span class="sxs-lookup"><span data-stu-id="98bb6-173">The password is the same as the deployment credentials created earlier in this guide.</span></span> <span data-ttu-id="98bb6-174">記錄檔可以是[串流處理到您使用 PowerShell 或 Azure CLI 的本機電腦直接](/azure/app-service/web-sites-enable-diagnostic-log#download)。</span><span class="sxs-lookup"><span data-stu-id="98bb6-174">The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#download).</span></span> <span data-ttu-id="98bb6-175">記錄檔也可以[Application Insights 中檢視](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights)。</span><span class="sxs-lookup"><span data-stu-id="98bb6-175">Logs can also be [viewed in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span></span>

## <a name="log-streaming"></a><span data-ttu-id="98bb6-176">記錄資料流</span><span class="sxs-lookup"><span data-stu-id="98bb6-176">Log streaming</span></span>

<span data-ttu-id="98bb6-177">透過入口網站的即時串流處理應用程式和 web 伺服器記錄檔。</span><span class="sxs-lookup"><span data-stu-id="98bb6-177">App and web server logs can be streamed in real time through the portal.</span></span>

1. <span data-ttu-id="98bb6-178">開啟[Azure 入口網站](https://portal.azure.com)，然後瀏覽至*mywebapp\<unique_number\>*  App Service。</span><span class="sxs-lookup"><span data-stu-id="98bb6-178">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="98bb6-179">在左側功能表中，向下捲動至**監視**區段，然後選取**記錄資料流**。</span><span class="sxs-lookup"><span data-stu-id="98bb6-179">In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.</span></span>

    ![螢幕擷取畫面顯示記錄檔資料流連結](./media/monitoring/log-stream.png)

<span data-ttu-id="98bb6-181">記錄檔也可以[透過 Azure CLI 或 Azure PowerShell 串流](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs)，包括透過 Cloud Shell。</span><span class="sxs-lookup"><span data-stu-id="98bb6-181">Logs can also be [streamed via Azure CLI or Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.</span></span>

## <a name="alerts"></a><span data-ttu-id="98bb6-182">警示</span><span class="sxs-lookup"><span data-stu-id="98bb6-182">Alerts</span></span>

<span data-ttu-id="98bb6-183">Azure 監視器 」 也提供[即時警示](/azure/monitoring-and-diagnostics/insights-alerts-portal)根據計量、 系統管理事件及其他準則。</span><span class="sxs-lookup"><span data-stu-id="98bb6-183">Azure Monitor also provides [real time alerts](/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.</span></span>

> <span data-ttu-id="98bb6-184">*注意：目前只有在警示 (傳統) 服務中才會提供 web 應用程式計量的警示。*</span><span class="sxs-lookup"><span data-stu-id="98bb6-184">*Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*</span></span>

<span data-ttu-id="98bb6-185">[警示 （傳統） 服務](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal)您可以在 Azure 監視器中或之下找到**監視**的 App Service 設定 區段。</span><span class="sxs-lookup"><span data-stu-id="98bb6-185">The [Alerts (classic) service](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.</span></span>

![警示 （傳統） 連結](./media/monitoring/alerts.png)

## <a name="live-debugging"></a><span data-ttu-id="98bb6-187">即時偵錯</span><span class="sxs-lookup"><span data-stu-id="98bb6-187">Live debugging</span></span>

<span data-ttu-id="98bb6-188">可以是 azure App Service[使用 Visual Studio 的遠端偵錯](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)當記錄檔未提供足夠的資訊。</span><span class="sxs-lookup"><span data-stu-id="98bb6-188">Azure App Service can be [debugged remotely with Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information.</span></span> <span data-ttu-id="98bb6-189">不過，遠端偵錯需要應用程式使用偵錯符號進行編譯。</span><span class="sxs-lookup"><span data-stu-id="98bb6-189">However, remote debugging requires the app to be compiled with debug symbols.</span></span> <span data-ttu-id="98bb6-190">偵錯不應該進行生產環境中，除了最後的手段。</span><span class="sxs-lookup"><span data-stu-id="98bb6-190">Debugging shouldn't be done in production, except as a last resort.</span></span>

## <a name="conclusion"></a><span data-ttu-id="98bb6-191">結論</span><span class="sxs-lookup"><span data-stu-id="98bb6-191">Conclusion</span></span>

<span data-ttu-id="98bb6-192">在本節中，您必須完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="98bb6-192">In this section, you completed the following tasks:</span></span>

* <span data-ttu-id="98bb6-193">尋找基本的監視和疑難排解在 Azure 入口網站中的資料</span><span class="sxs-lookup"><span data-stu-id="98bb6-193">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="98bb6-194">了解 Azure 監視器的所有 Azure 服務所提供的更深入的了解計量</span><span class="sxs-lookup"><span data-stu-id="98bb6-194">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="98bb6-195">使用 Application Insights 時使用的 web 應用程式連線的應用程式程式碼剖析</span><span class="sxs-lookup"><span data-stu-id="98bb6-195">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="98bb6-196">開啟記錄功能，並了解如何下載記錄檔</span><span class="sxs-lookup"><span data-stu-id="98bb6-196">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="98bb6-197">Stream 即時記錄檔</span><span class="sxs-lookup"><span data-stu-id="98bb6-197">Stream logs in real time</span></span>
* <span data-ttu-id="98bb6-198">了解如何設定警示</span><span class="sxs-lookup"><span data-stu-id="98bb6-198">Learn where to set up alerts</span></span>
* <span data-ttu-id="98bb6-199">深入了解遠端偵錯 Azure App Service web apps。</span><span class="sxs-lookup"><span data-stu-id="98bb6-199">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="98bb6-200">其他閱讀資料</span><span class="sxs-lookup"><span data-stu-id="98bb6-200">Additional reading</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="98bb6-201">監視 Azure web 應用程式效能，使用 Application Insights</span><span class="sxs-lookup"><span data-stu-id="98bb6-201">Monitor Azure web app performance with Application Insights</span></span>](/azure/application-insights/app-insights-azure-web-apps)
* [<span data-ttu-id="98bb6-202">為 Azure App Service 中的 Web 應用程式啟用診斷記錄</span><span class="sxs-lookup"><span data-stu-id="98bb6-202">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)
* [<span data-ttu-id="98bb6-203">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="98bb6-203">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="98bb6-204">Azure 監視器中建立傳統計量警示，Azure 服務-Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="98bb6-204">Create classic metric alerts in Azure Monitor for Azure services - Azure portal</span></span>](/azure/monitoring-and-diagnostics/insights-alerts-portal)
