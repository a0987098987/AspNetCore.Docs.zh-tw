---
title: 使用 ASP.NET Core 和 Azure 進行監視和 DevOps
author: CamSoper
description: 使用 ASP.NET Core 和 Azure，以 DevOps 解決方案的一部分監視和偵錯工具代碼
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 07/10/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: azure/devops/monitor
ms.openlocfilehash: a94b1e0b5ce2a24cf22eb665c9bcd03c25ffa67f
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85400371"
---
# <a name="monitor-and-debug"></a><span data-ttu-id="e71f4-103">監視和調試</span><span class="sxs-lookup"><span data-stu-id="e71f4-103">Monitor and debug</span></span>

<span data-ttu-id="e71f4-104">部署應用程式並建立 DevOps 管線之後，請務必瞭解如何監視應用程式並對其進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="e71f4-104">Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.</span></span>

<span data-ttu-id="e71f4-105">在本節中，您將完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="e71f4-105">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="e71f4-106">尋找 Azure 入口網站中的基本監視和疑難排解資料</span><span class="sxs-lookup"><span data-stu-id="e71f4-106">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="e71f4-107">瞭解 Azure 監視器如何讓您深入瞭解所有 Azure 服務的計量</span><span class="sxs-lookup"><span data-stu-id="e71f4-107">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="e71f4-108">將 web 應用程式與用於應用程式分析的 Application Insights 進行連線</span><span class="sxs-lookup"><span data-stu-id="e71f4-108">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="e71f4-109">開啟記錄功能，並瞭解下載記錄檔的位置</span><span class="sxs-lookup"><span data-stu-id="e71f4-109">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="e71f4-110">即時串流記錄</span><span class="sxs-lookup"><span data-stu-id="e71f4-110">Stream logs in real time</span></span>
* <span data-ttu-id="e71f4-111">瞭解設定警示的位置</span><span class="sxs-lookup"><span data-stu-id="e71f4-111">Learn where to set up alerts</span></span>
* <span data-ttu-id="e71f4-112">深入瞭解 Azure App Service web 應用程式的遠端偵錯程式。</span><span class="sxs-lookup"><span data-stu-id="e71f4-112">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="basic-monitoring-and-troubleshooting"></a><span data-ttu-id="e71f4-113">基本監視和疑難排解</span><span class="sxs-lookup"><span data-stu-id="e71f4-113">Basic monitoring and troubleshooting</span></span>

<span data-ttu-id="e71f4-114">App Service 的 web 應用程式會即時受到輕鬆監視。</span><span class="sxs-lookup"><span data-stu-id="e71f4-114">App Service web apps are easily monitored in real time.</span></span> <span data-ttu-id="e71f4-115">Azure 入口網站會以易於瞭解的圖表和圖形呈現計量。</span><span class="sxs-lookup"><span data-stu-id="e71f4-115">The Azure portal renders metrics in easy-to-understand charts and graphs.</span></span>

1. <span data-ttu-id="e71f4-116">開啟[Azure 入口網站](https://portal.azure.com)，然後流覽至\*mywebapp \<unique_number\> \* App Service。</span><span class="sxs-lookup"><span data-stu-id="e71f4-116">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>

1. <span data-ttu-id="e71f4-117">[**總覽**] 索引標籤會顯示有用的「一覽」資訊，包括顯示最近度量的圖形。</span><span class="sxs-lookup"><span data-stu-id="e71f4-117">The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.</span></span>

    ![顯示總覽面板的螢幕擷取畫面](./media/monitoring/overview.png)

    * <span data-ttu-id="e71f4-119">**Http 5xx**：伺服器端錯誤的計數，通常是 ASP.NET Core 程式碼中的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e71f4-119">**Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.</span></span>
    * <span data-ttu-id="e71f4-120">**中的資料**：資料輸入進入您的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e71f4-120">**Data In**: Data ingress coming into your web app.</span></span>
    * <span data-ttu-id="e71f4-121">**資料輸出**：從 web 應用程式到用戶端的資料輸出。</span><span class="sxs-lookup"><span data-stu-id="e71f4-121">**Data Out**: Data egress from your web app to clients.</span></span>
    * <span data-ttu-id="e71f4-122">**要求**： HTTP 要求的計數。</span><span class="sxs-lookup"><span data-stu-id="e71f4-122">**Requests**: Count of HTTP requests.</span></span>
    * <span data-ttu-id="e71f4-123">**平均回應時間**： web 應用程式回應 HTTP 要求的平均時間。</span><span class="sxs-lookup"><span data-stu-id="e71f4-123">**Average Response Time**: Average time for the web app to respond to HTTP requests.</span></span>

    <span data-ttu-id="e71f4-124">您也可以在此頁面上找到數個用於疑難排解和優化的自助工具。</span><span class="sxs-lookup"><span data-stu-id="e71f4-124">Several self-service tools for troubleshooting and optimization are also found on this page.</span></span>

    ![顯示自助服務工具的螢幕擷取畫面](./media/monitoring/wizards.png)

    * <span data-ttu-id="e71f4-126">**診斷並解決問題**是自助服務的疑難排解員。</span><span class="sxs-lookup"><span data-stu-id="e71f4-126">**Diagnose and solve problems** is a self-service troubleshooter.</span></span>
    * <span data-ttu-id="e71f4-127">**Application Insights**適用于分析效能和應用程式行為，稍後將在本節中討論。</span><span class="sxs-lookup"><span data-stu-id="e71f4-127">**Application Insights** is for profiling performance and app behavior, and is discussed later in this section.</span></span>
    * <span data-ttu-id="e71f4-128">**App Service Advisor**提出建議來調整您的應用程式體驗。</span><span class="sxs-lookup"><span data-stu-id="e71f4-128">**App Service Advisor** makes recommendations to tune your app experience.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="e71f4-129">進階監視</span><span class="sxs-lookup"><span data-stu-id="e71f4-129">Advanced monitoring</span></span>

<span data-ttu-id="e71f4-130">[Azure 監視器](/azure/monitoring-and-diagnostics/)是集中式服務，可跨 Azure 服務監視所有計量和設定警示。</span><span class="sxs-lookup"><span data-stu-id="e71f4-130">[Azure Monitor](/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services.</span></span> <span data-ttu-id="e71f4-131">在 Azure 監視器內，系統管理員可以精細地追蹤效能並找出趨勢。</span><span class="sxs-lookup"><span data-stu-id="e71f4-131">Within Azure Monitor, administrators can granularly track performance and identify trends.</span></span> <span data-ttu-id="e71f4-132">每個 Azure 服務都提供自己[的一組計量](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions)來 Azure 監視器。</span><span class="sxs-lookup"><span data-stu-id="e71f4-132">Each Azure service offers its own [set of metrics](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.</span></span>

## <a name="profile-with-application-insights"></a><span data-ttu-id="e71f4-133">使用 Application Insights 的設定檔</span><span class="sxs-lookup"><span data-stu-id="e71f4-133">Profile with Application Insights</span></span>

<span data-ttu-id="e71f4-134">[Application Insights](/azure/application-insights/app-insights-overview)是一項 Azure 服務，可用於分析 web 應用程式的效能和穩定性，以及使用者使用它們的方式。</span><span class="sxs-lookup"><span data-stu-id="e71f4-134">[Application Insights](/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them.</span></span> <span data-ttu-id="e71f4-135">Application Insights 的資料比 Azure 監視器更廣且更深入。</span><span class="sxs-lookup"><span data-stu-id="e71f4-135">The data from Application Insights is broader and deeper than that of Azure Monitor.</span></span> <span data-ttu-id="e71f4-136">資料可為開發人員和系統管理員提供改善應用程式的重要資訊。</span><span class="sxs-lookup"><span data-stu-id="e71f4-136">The data can provide developers and administrators with key information for improving apps.</span></span> <span data-ttu-id="e71f4-137">Application Insights 可以新增至 Azure App Service 資源，而不需要變更程式碼。</span><span class="sxs-lookup"><span data-stu-id="e71f4-137">Application Insights can be added to an Azure App Service resource without code changes.</span></span>

1. <span data-ttu-id="e71f4-138">開啟[Azure 入口網站](https://portal.azure.com)，然後流覽至\*mywebapp \<unique_number\> \* App Service。</span><span class="sxs-lookup"><span data-stu-id="e71f4-138">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="e71f4-139">從 [**總覽**] 索引標籤中，按一下 [ **Application Insights** ] 磚。</span><span class="sxs-lookup"><span data-stu-id="e71f4-139">From the **Overview** tab, click the **Application Insights** tile.</span></span>

    ![Application Insights 圖格](./media/monitoring/app-insights.png)

1. <span data-ttu-id="e71f4-141">選取 [**建立新資源**] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="e71f4-141">Select the **Create new resource** radio button.</span></span> <span data-ttu-id="e71f4-142">使用預設的資源名稱，然後選取 Application Insights 資源的位置。</span><span class="sxs-lookup"><span data-stu-id="e71f4-142">Use the default resource name, and select the location for the Application Insights resource.</span></span> <span data-ttu-id="e71f4-143">位置不需要與您的 web 應用程式相符。</span><span class="sxs-lookup"><span data-stu-id="e71f4-143">The location doesn't need to match that of your web app.</span></span>

    ![Application Insights 設定](./media/monitoring/new-app-insights.png)

1. <span data-ttu-id="e71f4-145">針對 [**執行時間/架構**]，選取 [ **ASP.NET Core**]。</span><span class="sxs-lookup"><span data-stu-id="e71f4-145">For **Runtime/Framework**, select **ASP.NET Core**.</span></span> <span data-ttu-id="e71f4-146">接受預設設定。</span><span class="sxs-lookup"><span data-stu-id="e71f4-146">Accept the default settings.</span></span>
1. <span data-ttu-id="e71f4-147">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e71f4-147">Select **OK**.</span></span> <span data-ttu-id="e71f4-148">如果系統提示您確認，請選取 [**繼續**]。</span><span class="sxs-lookup"><span data-stu-id="e71f4-148">If prompted to confirm, select **Continue**.</span></span>
1. <span data-ttu-id="e71f4-149">建立資源之後，請按一下 Application Insights 資源的名稱，直接流覽至 [Application Insights] 頁面。</span><span class="sxs-lookup"><span data-stu-id="e71f4-149">After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.</span></span>

    ![已準備好新的 Application Insights 資源](./media/monitoring/new-app-insights-done.png)

<span data-ttu-id="e71f4-151">使用應用程式時，資料會累積。</span><span class="sxs-lookup"><span data-stu-id="e71f4-151">As the app is used, data accumulates.</span></span> <span data-ttu-id="e71f4-152">選取 **[** 重新整理]，以使用新的資料重載此分頁。</span><span class="sxs-lookup"><span data-stu-id="e71f4-152">Select **Refresh** to reload the blade with new data.</span></span>

![Application Insights 總覽] 索引標籤](./media/monitoring/app-insights-overview.png)

<span data-ttu-id="e71f4-154">Application Insights 提供有用的伺服器端資訊，而不需進行其他設定。</span><span class="sxs-lookup"><span data-stu-id="e71f4-154">Application Insights provides useful server-side information with no additional configuration.</span></span> <span data-ttu-id="e71f4-155">若要充分發揮 Application Insights 的價值，請[使用 APPLICATION INSIGHTS SDK 檢測您的應用程式](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="e71f4-155">To get the most value from Application Insights, [instrument your app with the Application Insights SDK](/azure/application-insights/app-insights-asp-net-core).</span></span> <span data-ttu-id="e71f4-156">當設定正確時，服務會跨網頁伺服器和瀏覽器提供端對端監視，包括用戶端效能。</span><span class="sxs-lookup"><span data-stu-id="e71f4-156">When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance.</span></span> <span data-ttu-id="e71f4-157">如需詳細資訊，請參閱[Application Insights 檔](/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="e71f4-157">For more information, see the [Application Insights documentation](/azure/application-insights/app-insights-overview).</span></span>

## <a name="logging"></a><span data-ttu-id="e71f4-158">記錄</span><span class="sxs-lookup"><span data-stu-id="e71f4-158">Logging</span></span>

<span data-ttu-id="e71f4-159">在 Azure App Service 中，預設會停用 Web 服務器和應用程式記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e71f4-159">Web server and app logs are disabled by default in Azure App Service.</span></span> <span data-ttu-id="e71f4-160">使用下列步驟啟用記錄：</span><span class="sxs-lookup"><span data-stu-id="e71f4-160">Enable the logs with the following steps:</span></span>

1. <span data-ttu-id="e71f4-161">開啟[Azure 入口網站](https://portal.azure.com)，然後流覽至\*mywebapp \<unique_number\> \* App Service。</span><span class="sxs-lookup"><span data-stu-id="e71f4-161">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="e71f4-162">在左側功能表中，向下流覽至 [**監視**] 區段。</span><span class="sxs-lookup"><span data-stu-id="e71f4-162">In the menu to the left, scroll down to the **Monitoring** section.</span></span> <span data-ttu-id="e71f4-163">選取 [**診斷記錄**]。</span><span class="sxs-lookup"><span data-stu-id="e71f4-163">Select **Diagnostics logs**.</span></span>

    ![診斷記錄連結](./media/monitoring/logging.png)

1. <span data-ttu-id="e71f4-165">開啟 **[應用程式記錄（檔案系統）**]。</span><span class="sxs-lookup"><span data-stu-id="e71f4-165">Turn on **Application Logging (Filesystem)**.</span></span> <span data-ttu-id="e71f4-166">若出現提示，請按一下方塊以安裝延伸模組，以在 web 應用程式中啟用應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="e71f4-166">If prompted, click the box to install the extensions to enable app logging in the web app.</span></span>
1. <span data-ttu-id="e71f4-167">將 [ **Web 服務器記錄**] 設定為 [**檔案系統**]。</span><span class="sxs-lookup"><span data-stu-id="e71f4-167">Set **Web server logging** to **File System**.</span></span>
1. <span data-ttu-id="e71f4-168">輸入**保留期限**（以天為單位）。</span><span class="sxs-lookup"><span data-stu-id="e71f4-168">Enter the **Retention Period** in days.</span></span> <span data-ttu-id="e71f4-169">例如，30。</span><span class="sxs-lookup"><span data-stu-id="e71f4-169">For example, 30.</span></span>
1. <span data-ttu-id="e71f4-170">按一下 [檔案] \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="e71f4-170">Click **Save**.</span></span>

<span data-ttu-id="e71f4-171">ASP.NET Core 和 web 伺服器（App Service）記錄會針對 web 應用程式產生。</span><span class="sxs-lookup"><span data-stu-id="e71f4-171">ASP.NET Core and web server (App Service) logs are generated for the web app.</span></span> <span data-ttu-id="e71f4-172">您可以使用顯示的 FTP/FTPS 資訊來下載它們。</span><span class="sxs-lookup"><span data-stu-id="e71f4-172">They can be downloaded using the FTP/FTPS information displayed.</span></span> <span data-ttu-id="e71f4-173">此密碼與本指南稍早建立的部署認證相同。</span><span class="sxs-lookup"><span data-stu-id="e71f4-173">The password is the same as the deployment credentials created earlier in this guide.</span></span> <span data-ttu-id="e71f4-174">[您可以使用 PowerShell 或 Azure CLI，將記錄直接串流至您的本機電腦](/azure/app-service/web-sites-enable-diagnostic-log#download)。</span><span class="sxs-lookup"><span data-stu-id="e71f4-174">The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#download).</span></span> <span data-ttu-id="e71f4-175">您也可以[在 Application Insights 中查看](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights)記錄。</span><span class="sxs-lookup"><span data-stu-id="e71f4-175">Logs can also be [viewed in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span></span>

## <a name="log-streaming"></a><span data-ttu-id="e71f4-176">記錄串流</span><span class="sxs-lookup"><span data-stu-id="e71f4-176">Log streaming</span></span>

<span data-ttu-id="e71f4-177">應用程式和 web 伺服器記錄可以透過入口網站即時串流。</span><span class="sxs-lookup"><span data-stu-id="e71f4-177">App and web server logs can be streamed in real time through the portal.</span></span>

1. <span data-ttu-id="e71f4-178">開啟[Azure 入口網站](https://portal.azure.com)，然後流覽至\*mywebapp \<unique_number\> \* App Service。</span><span class="sxs-lookup"><span data-stu-id="e71f4-178">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="e71f4-179">在左側功能表中，向下流覽至 [**監視**] 區段，然後選取 [**記錄資料流程**]。</span><span class="sxs-lookup"><span data-stu-id="e71f4-179">In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.</span></span>

    ![顯示記錄資料流程連結的螢幕擷取畫面](./media/monitoring/log-stream.png)

<span data-ttu-id="e71f4-181">記錄也可以透過[Azure CLI 或 Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs)進行串流處理，包括 Cloud Shell。</span><span class="sxs-lookup"><span data-stu-id="e71f4-181">Logs can also be [streamed via Azure CLI or Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.</span></span>

## <a name="alerts"></a><span data-ttu-id="e71f4-182">警示</span><span class="sxs-lookup"><span data-stu-id="e71f4-182">Alerts</span></span>

<span data-ttu-id="e71f4-183">Azure 監視器也會根據計量、系統管理事件和其他準則提供[即時警示](/azure/monitoring-and-diagnostics/insights-alerts-portal)。</span><span class="sxs-lookup"><span data-stu-id="e71f4-183">Azure Monitor also provides [real time alerts](/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.</span></span>

> <span data-ttu-id="e71f4-184">*注意：目前只有在 [警示（傳統）] 服務中才會提供 web 應用程式計量的警示。*</span><span class="sxs-lookup"><span data-stu-id="e71f4-184">*Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*</span></span>

<span data-ttu-id="e71f4-185">您可以在 Azure 監視器或 App Service 設定的 [**監視**] 區段下找到 [[警示（傳統）] 服務](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal)。</span><span class="sxs-lookup"><span data-stu-id="e71f4-185">The [Alerts (classic) service](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.</span></span>

![警示（傳統）連結](./media/monitoring/alerts.png)

## <a name="live-debugging"></a><span data-ttu-id="e71f4-187">即時調試</span><span class="sxs-lookup"><span data-stu-id="e71f4-187">Live debugging</span></span>

<span data-ttu-id="e71f4-188">當記錄檔未提供足夠的資訊時[，可使用 Visual Studio 從遠端進行 Azure App Service 的調試](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)。</span><span class="sxs-lookup"><span data-stu-id="e71f4-188">Azure App Service can be [debugged remotely with Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information.</span></span> <span data-ttu-id="e71f4-189">不過，遠端偵錯程式需要使用 debug 符號來編譯應用程式。</span><span class="sxs-lookup"><span data-stu-id="e71f4-189">However, remote debugging requires the app to be compiled with debug symbols.</span></span> <span data-ttu-id="e71f4-190">除了最後的手段以外，不應在生產環境中執行調試。</span><span class="sxs-lookup"><span data-stu-id="e71f4-190">Debugging shouldn't be done in production, except as a last resort.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e71f4-191">結論</span><span class="sxs-lookup"><span data-stu-id="e71f4-191">Conclusion</span></span>

<span data-ttu-id="e71f4-192">在本節中，您已完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="e71f4-192">In this section, you completed the following tasks:</span></span>

* <span data-ttu-id="e71f4-193">尋找 Azure 入口網站中的基本監視和疑難排解資料</span><span class="sxs-lookup"><span data-stu-id="e71f4-193">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="e71f4-194">瞭解 Azure 監視器如何讓您深入瞭解所有 Azure 服務的計量</span><span class="sxs-lookup"><span data-stu-id="e71f4-194">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="e71f4-195">將 web 應用程式與用於應用程式分析的 Application Insights 進行連線</span><span class="sxs-lookup"><span data-stu-id="e71f4-195">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="e71f4-196">開啟記錄功能，並瞭解下載記錄檔的位置</span><span class="sxs-lookup"><span data-stu-id="e71f4-196">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="e71f4-197">即時串流記錄</span><span class="sxs-lookup"><span data-stu-id="e71f4-197">Stream logs in real time</span></span>
* <span data-ttu-id="e71f4-198">瞭解設定警示的位置</span><span class="sxs-lookup"><span data-stu-id="e71f4-198">Learn where to set up alerts</span></span>
* <span data-ttu-id="e71f4-199">深入瞭解 Azure App Service web 應用程式的遠端偵錯程式。</span><span class="sxs-lookup"><span data-stu-id="e71f4-199">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="e71f4-200">其他閱讀資料</span><span class="sxs-lookup"><span data-stu-id="e71f4-200">Additional reading</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="e71f4-201">使用 Application Insights 監視 Azure Web 應用程式效能</span><span class="sxs-lookup"><span data-stu-id="e71f4-201">Monitor Azure web app performance with Application Insights</span></span>](/azure/application-insights/app-insights-azure-web-apps)
* [<span data-ttu-id="e71f4-202">在 Azure App Service 中針對 Web 應用程式啟用診斷記錄功能。</span><span class="sxs-lookup"><span data-stu-id="e71f4-202">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)
* [<span data-ttu-id="e71f4-203">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="e71f4-203">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="e71f4-204">在 Azure 監視器中為 Azure 服務建立傳統計量警示 - Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="e71f4-204">Create classic metric alerts in Azure Monitor for Azure services - Azure portal</span></span>](/azure/monitoring-and-diagnostics/insights-alerts-portal)
