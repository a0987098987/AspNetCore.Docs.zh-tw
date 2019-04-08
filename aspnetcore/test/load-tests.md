---
title: ASP.NET Core 負載/壓力測試
author: Jeremy-Meng
description: 了解幾個值得注意的工具和負載測試和壓力測試 ASP.NET Core 應用程式的方法。
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 0a8449ea2c9df0f2ac93058f03af0a1a2aa66508
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068179"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="c8a41-103">ASP.NET Core 負載/壓力測試</span><span class="sxs-lookup"><span data-stu-id="c8a41-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="c8a41-104">負載測試和壓力測試很重要，可確保 web 應用程式是高效能且可調整。</span><span class="sxs-lookup"><span data-stu-id="c8a41-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="c8a41-105">其目標是不同的即使它們通常會共用類似的測試。</span><span class="sxs-lookup"><span data-stu-id="c8a41-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="c8a41-106">**負載測試**&ndash;測試是否應用程式可以處理的特定案例的使用者指定的負載，同時仍然可滿足回應目標。</span><span class="sxs-lookup"><span data-stu-id="c8a41-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="c8a41-107">在正常情況下執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8a41-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="c8a41-108">**壓力測試**&ndash;極端的條件，通常很長一段時間下執行時，測試應用程式穩定性。</span><span class="sxs-lookup"><span data-stu-id="c8a41-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="c8a41-109">測試高使用者負載尖峰或逐漸增加的負載，對應用程式，或者限制應用程式的運算資源。</span><span class="sxs-lookup"><span data-stu-id="c8a41-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="c8a41-110">壓力測試會判斷是否在壓力下的應用程式可以從失敗中復原，並依正常程序傳回給預期的行為。</span><span class="sxs-lookup"><span data-stu-id="c8a41-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="c8a41-111">壓力的情況下，不會在正常情況下執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8a41-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="c8a41-112">Visual Studio 2019 是最後一個版本的 Visual Studio 負載測試功能。</span><span class="sxs-lookup"><span data-stu-id="c8a41-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="c8a41-113">對於需要負載測試工具在未來的客戶，我們建議替代工具，例如 Apache JMeter、 Akamai CloudTest 和 BlazeMeter。</span><span class="sxs-lookup"><span data-stu-id="c8a41-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="c8a41-114">如需詳細資訊，請參閱 < [Visual Studio 2019 版本資訊](/visualstudio/releases/2019/release-notes#test-tools)。</span><span class="sxs-lookup"><span data-stu-id="c8a41-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes#test-tools).</span></span>

<span data-ttu-id="c8a41-115">Azure DevOps 中測試服務的負載即將結束在 2020年。</span><span class="sxs-lookup"><span data-stu-id="c8a41-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="c8a41-116">如需詳細資訊，請參閱 <<c0> [ 雲端式負載測試服務生命週期結束的](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/)。</span><span class="sxs-lookup"><span data-stu-id="c8a41-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="c8a41-117">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="c8a41-117">Visual Studio tools</span></span>

<span data-ttu-id="c8a41-118">Visual Studio 可讓使用者建立、 開發及偵錯 web 效能和負載測試。</span><span class="sxs-lookup"><span data-stu-id="c8a41-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="c8a41-119">會提供透過網頁瀏覽器中錄製動作建立測試的選項。</span><span class="sxs-lookup"><span data-stu-id="c8a41-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="c8a41-120">如需如何建立、 設定和執行負載測試使用 Visual Studio 2017 的專案資訊，請參閱[快速入門：建立負載測試專案](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)。</span><span class="sxs-lookup"><span data-stu-id="c8a41-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span> <span data-ttu-id="c8a41-121">如需詳細資訊，請參閱 <<c0> [ 額外的資源](#additional-resources)一節。</span><span class="sxs-lookup"><span data-stu-id="c8a41-121">For more information, see the [Additional resources](#additional-resources) section.</span></span>

<span data-ttu-id="c8a41-122">若要使用 Azure DevOps 的雲端中執行內部部署或執行，您可以設定負載測試。</span><span class="sxs-lookup"><span data-stu-id="c8a41-122">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="c8a41-123">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="c8a41-123">Azure DevOps</span></span>

<span data-ttu-id="c8a41-124">可以使用啟動負載測試回合[Azure DevOps 測試計劃](/azure/devops/test/load-test/index?view=vsts)服務。</span><span class="sxs-lookup"><span data-stu-id="c8a41-124">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Azure 的 DevOps 負載測試登陸頁面](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="c8a41-126">服務支援下列測試格式：</span><span class="sxs-lookup"><span data-stu-id="c8a41-126">The service supports the following test formats:</span></span>

* <span data-ttu-id="c8a41-127">Visual Studio &ndash; Visual Studio 中建立的 Web 測試。</span><span class="sxs-lookup"><span data-stu-id="c8a41-127">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="c8a41-128">HTTP 封存&ndash;封存內的擷取 HTTP 流量會重新執行測試期間。</span><span class="sxs-lookup"><span data-stu-id="c8a41-128">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="c8a41-129">[URL 型](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts)&ndash;可讓您指定負載測試中，要求類型、 標頭和查詢字串的 Url。</span><span class="sxs-lookup"><span data-stu-id="c8a41-129">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="c8a41-130">執行設定的參數，例如持續時間，負載模式和使用者人數可以設定。</span><span class="sxs-lookup"><span data-stu-id="c8a41-130">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="c8a41-131">[Apache JMeter](https://jmeter.apache.org/)。</span><span class="sxs-lookup"><span data-stu-id="c8a41-131">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="c8a41-132">Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="c8a41-132">Azure portal</span></span>

<span data-ttu-id="c8a41-133">[設定及執行負載測試的 web 應用程式，可讓 azure 入口網站](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts)直接從**效能**在 Azure 入口網站中的應用程式服務 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="c8a41-133">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![在 Azure 入口網站中的 azure App Service](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="c8a41-135">測試可以是具有指定的 URL 或 Visual Studio Web 測試檔案，其中可以測試多個 Url 的手動測試。</span><span class="sxs-lookup"><span data-stu-id="c8a41-135">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![在 Azure 入口網站上新的效能測試頁面](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="c8a41-137">在測試結束時，產生的報表會顯示應用程式的效能特性。</span><span class="sxs-lookup"><span data-stu-id="c8a41-137">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="c8a41-138">範例統計資料包括：</span><span class="sxs-lookup"><span data-stu-id="c8a41-138">Example statistics include:</span></span>

* <span data-ttu-id="c8a41-139">平均回應時間</span><span class="sxs-lookup"><span data-stu-id="c8a41-139">Average response time</span></span>
* <span data-ttu-id="c8a41-140">最大輸送量： 每秒要求數</span><span class="sxs-lookup"><span data-stu-id="c8a41-140">Max throughput: requests per second</span></span>
* <span data-ttu-id="c8a41-141">失敗百分比</span><span class="sxs-lookup"><span data-stu-id="c8a41-141">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="c8a41-142">第三方工具</span><span class="sxs-lookup"><span data-stu-id="c8a41-142">Third-party tools</span></span>

<span data-ttu-id="c8a41-143">下列清單包含使用各種不同的功能集的第三方 web 效能工具：</span><span class="sxs-lookup"><span data-stu-id="c8a41-143">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="c8a41-144">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="c8a41-144">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="c8a41-145">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="c8a41-145">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="c8a41-146">Gatling</span><span class="sxs-lookup"><span data-stu-id="c8a41-146">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="c8a41-147">Locust</span><span class="sxs-lookup"><span data-stu-id="c8a41-147">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="c8a41-148">West Wind WebSurge</span><span class="sxs-lookup"><span data-stu-id="c8a41-148">West Wind WebSurge</span></span>](http://websurge.west-wind.com/)
* [<span data-ttu-id="c8a41-149">Netling</span><span class="sxs-lookup"><span data-stu-id="c8a41-149">Netling</span></span>](https://github.com/hallatore/Netling)

## <a name="additional-resources"></a><span data-ttu-id="c8a41-150">其他資源</span><span class="sxs-lookup"><span data-stu-id="c8a41-150">Additional resources</span></span>

* [<span data-ttu-id="c8a41-151">載入測試部落格系列</span><span class="sxs-lookup"><span data-stu-id="c8a41-151">Load Test blog series</span></span>](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
