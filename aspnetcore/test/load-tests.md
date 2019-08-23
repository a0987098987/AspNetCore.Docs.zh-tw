---
title: ASP.NET Core 負載/壓力測試
author: Jeremy-Meng
description: 瞭解 ASP.NET Core 應用程式的負載測試和壓力測試的幾項值得注意的工具和方法。
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 7a9dfc1fedf747ab26daa573b61ed01c31709058
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975238"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="a7fd7-103">ASP.NET Core 負載/壓力測試</span><span class="sxs-lookup"><span data-stu-id="a7fd7-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="a7fd7-104">負載測試和壓力測試非常重要, 可確保 web 應用程式的效能和可擴充性。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="a7fd7-105">它們的目標會不同, 即使它們經常共用類似的測試也一樣。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="a7fd7-106">**負載測試**&ndash;測試應用程式是否可以在特定案例中處理指定的使用者負載, 同時仍然滿足回應目標。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="a7fd7-107">應用程式會在正常情況下執行。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="a7fd7-108">**壓力測試**&ndash;在極端情況下執行時, 測試應用程式的穩定性通常長時間。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="a7fd7-109">測試會在應用程式上放置高使用者負載、尖峰或逐漸增加的負載, 或限制應用程式的運算資源。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="a7fd7-110">壓力測試會判斷壓力不足的應用程式是否可以從失敗中復原, 並正常地回到預期的行為。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="a7fd7-111">在壓力之下, 應用程式不會在正常情況下執行。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="a7fd7-112">Visual Studio 2019 是具有負載測試功能 Visual Studio 的最後一個版本。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="a7fd7-113">對於未來需要負載測試工具的客戶, 我們建議替代工具, 例如 Apache JMeter、Akamai CloudTest 和 BlazeMeter。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="a7fd7-114">如需詳細資訊, 請參閱[Visual Studio 2019 版本](/visualstudio/releases/2019/release-notes-v16.0#test-tools)資訊。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

<span data-ttu-id="a7fd7-115">Azure DevOps 中的負載測試服務將于2020結束。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="a7fd7-116">如需詳細資訊, 請參閱[雲端負載測試服務生命週期結束](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/)。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="a7fd7-117">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="a7fd7-117">Visual Studio tools</span></span>

<span data-ttu-id="a7fd7-118">Visual Studio 可讓使用者建立、開發和調試 web 效能和負載測試。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="a7fd7-119">有一個選項可用來在網頁瀏覽器中錄製動作, 以建立測試。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="a7fd7-120">如需如何使用 Visual Studio 2017 建立、設定及執行負載測試專案的詳細資訊, 請參閱[快速入門:建立負載測試專案](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="a7fd7-121">負載測試可以設定為在內部部署執行, 或使用 Azure DevOps 在雲端中執行。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-121">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="a7fd7-122">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="a7fd7-122">Azure DevOps</span></span>

<span data-ttu-id="a7fd7-123">您可以使用[Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts)服務來啟動負載測試回合。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-123">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Azure DevOps 負載測試登陸頁面](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="a7fd7-125">此服務支援下列測試格式:</span><span class="sxs-lookup"><span data-stu-id="a7fd7-125">The service supports the following test formats:</span></span>

* <span data-ttu-id="a7fd7-126">Visual Studio &ndash;在 Visual Studio 中建立的 Web 測試。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-126">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="a7fd7-127">在測試期間, 將會重新執行 HTTP 封存在封存內捕獲的HTTP流量。&ndash;</span><span class="sxs-lookup"><span data-stu-id="a7fd7-127">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="a7fd7-128">以[URL 為基礎](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts)&ndash;允許指定 url 來載入測試、要求類型、標頭和查詢字串。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-128">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="a7fd7-129">您可以設定執行設定參數, 例如持續時間、負載模式和使用者數目。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-129">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="a7fd7-130">[Apache JMeter](https://jmeter.apache.org/)。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-130">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="a7fd7-131">Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="a7fd7-131">Azure portal</span></span>

<span data-ttu-id="a7fd7-132">Azure 入口網站允許直接從 Azure 入口網站中 App Service 的 [**效能**] 索引標籤[, 設定和執行 web 應用程式的負載測試](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts)。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-132">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Azure 入口網站中的 Azure App Service](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="a7fd7-134">測試可以是具有指定 URL 或 Visual Studio Web 測試檔案的手動測試, 可以測試多個 Url。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-134">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Azure 入口網站上的 [新增效能測試] 頁面](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="a7fd7-136">在測試結束時, 產生的報表會顯示應用程式的效能特性。</span><span class="sxs-lookup"><span data-stu-id="a7fd7-136">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="a7fd7-137">範例統計資料包括:</span><span class="sxs-lookup"><span data-stu-id="a7fd7-137">Example statistics include:</span></span>

* <span data-ttu-id="a7fd7-138">平均回應時間</span><span class="sxs-lookup"><span data-stu-id="a7fd7-138">Average response time</span></span>
* <span data-ttu-id="a7fd7-139">輸送量上限: 每秒的要求數</span><span class="sxs-lookup"><span data-stu-id="a7fd7-139">Max throughput: requests per second</span></span>
* <span data-ttu-id="a7fd7-140">失敗百分比</span><span class="sxs-lookup"><span data-stu-id="a7fd7-140">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="a7fd7-141">協力廠商工具</span><span class="sxs-lookup"><span data-stu-id="a7fd7-141">Third-party tools</span></span>

<span data-ttu-id="a7fd7-142">下列清單包含具有各種功能集的協力廠商 web 效能工具:</span><span class="sxs-lookup"><span data-stu-id="a7fd7-142">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="a7fd7-143">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="a7fd7-143">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="a7fd7-144">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="a7fd7-144">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="a7fd7-145">Gatling</span><span class="sxs-lookup"><span data-stu-id="a7fd7-145">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="a7fd7-146">Locust</span><span class="sxs-lookup"><span data-stu-id="a7fd7-146">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="a7fd7-147">West 風 WebSurge</span><span class="sxs-lookup"><span data-stu-id="a7fd7-147">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="a7fd7-148">Netling</span><span class="sxs-lookup"><span data-stu-id="a7fd7-148">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="a7fd7-149">Vegeta</span><span class="sxs-lookup"><span data-stu-id="a7fd7-149">Vegeta</span></span>](https://github.com/tsenart/vegeta)
