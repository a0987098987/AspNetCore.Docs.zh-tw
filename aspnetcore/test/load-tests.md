---
title: ASP.NET Core 負載/壓力測試
author: Jeremy-Meng
description: 描述幾個值得注意的工具和負載測試和壓力測試 ASP.NET Core 應用程式的方法。
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 587df6e216943d3eeec779df4d0554dd0fc2fda0
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345424"
---
# <a name="load-and-stress-testing-aspnet-core"></a><span data-ttu-id="a8e15-103">負載和壓力測試的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8e15-103">Load and stress testing ASP.NET Core</span></span>

<span data-ttu-id="a8e15-104">負載測試和壓力測試很重要，可確保 web 應用程式是高效能且可調整。</span><span class="sxs-lookup"><span data-stu-id="a8e15-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="a8e15-105">其目標是不同甚至是他們通常會共用類似的測試。</span><span class="sxs-lookup"><span data-stu-id="a8e15-105">Their goals are different even they often share similar tests.</span></span>

<span data-ttu-id="a8e15-106">**負載測試**:測試是否應用程式可以處理的特定案例的使用者指定的負載，同時仍然可滿足回應目標。</span><span class="sxs-lookup"><span data-stu-id="a8e15-106">**Load tests**: Tests whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="a8e15-107">在正常情況下執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e15-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="a8e15-108">**壓力測試**:測試應用程式穩定性下執行，就極端的情況，而且通常很長一段時間時：</span><span class="sxs-lookup"><span data-stu-id="a8e15-108">**Stress tests**: Tests app stability when running under extreme conditions and often a long period of time:</span></span>

* <span data-ttu-id="a8e15-109">高使用者負載 – 尖峰或逐漸增加。</span><span class="sxs-lookup"><span data-stu-id="a8e15-109">High user load – either spikes or gradually increasing.</span></span>
* <span data-ttu-id="a8e15-110">有限的運算資源。</span><span class="sxs-lookup"><span data-stu-id="a8e15-110">Limited computing resources.</span></span>  

<span data-ttu-id="a8e15-111">壓力的情況下，可以應用程式從失敗中復原並依正常程序傳回預期的行為？</span><span class="sxs-lookup"><span data-stu-id="a8e15-111">Under stress, can the app recover from failure and gracefully return to expected behavior?</span></span> <span data-ttu-id="a8e15-112">應用程式是在資源不足時*不*在正常情況下執行。</span><span class="sxs-lookup"><span data-stu-id="a8e15-112">Under stress, the app is *not* run under normal conditions.</span></span>

<span data-ttu-id="a8e15-113">Visual Studio 2019 將會是具有負載測試功能的最終 Visual Studio 版本。</span><span class="sxs-lookup"><span data-stu-id="a8e15-113">Visual Studio 2019 will be the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="a8e15-114">針對需要負載測試工具的客戶，我們建議使用替代的負載測試工具，例如 Apache JMeter、Akamai CloudTest、Blazemeter。</span><span class="sxs-lookup"><span data-stu-id="a8e15-114">For customers requiring load testing tools, we recommend using alternate load testing tools such as Apache JMeter, Akamai CloudTest, Blazemeter.</span></span> <span data-ttu-id="a8e15-115">如需詳細資訊，請參閱 < [Visual Studio 2019 Preview 版本資訊](/visualstudio/releases/2019/release-notes-preview#test-tools)。</span><span class="sxs-lookup"><span data-stu-id="a8e15-115">For more information, see the [Visual Studio 2019 Preview Release Notes](/visualstudio/releases/2019/release-notes-preview#test-tools).</span></span>

<span data-ttu-id="a8e15-116">Azure DevOps 中測試服務的負載即將結束在 2020年。</span><span class="sxs-lookup"><span data-stu-id="a8e15-116">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="a8e15-117">如需詳細資訊，請參閱[雲端式負載測試服務生命週期結束的](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/)。</span><span class="sxs-lookup"><span data-stu-id="a8e15-117">For more information see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="a8e15-118">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="a8e15-118">Visual Studio Tools</span></span>

<span data-ttu-id="a8e15-119">Visual Studio 可讓使用者建立、 開發及偵錯 web 效能和負載測試。</span><span class="sxs-lookup"><span data-stu-id="a8e15-119">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="a8e15-120">會提供透過網頁瀏覽器中錄製動作建立測試的選項。</span><span class="sxs-lookup"><span data-stu-id="a8e15-120">An option is available to create tests by recording actions in web browser.</span></span>

<span data-ttu-id="a8e15-121">[快速入門：建立負載測試專案](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)示範如何建立、 設定和執行負載測試使用 Visual Studio 2017 的專案。</span><span class="sxs-lookup"><span data-stu-id="a8e15-121">[Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) shows how to create, configure, and run a load test projects using Visual Studio 2017.</span></span>

<span data-ttu-id="a8e15-122">如需詳細資訊，請參閱[其他資源](#add)。</span><span class="sxs-lookup"><span data-stu-id="a8e15-122">See [Additional Resources](#add) for more information.</span></span>

<span data-ttu-id="a8e15-123">在內部部署中執行，或使用 Azure DevOps 的雲端中執行，您可以設定負載測試。</span><span class="sxs-lookup"><span data-stu-id="a8e15-123">Load tests can be configured to run in on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="a8e15-124">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="a8e15-124">Azure DevOps</span></span>

<span data-ttu-id="a8e15-125">可以使用啟動負載測試回合[Azure DevOps 測試計劃](/azure/devops/test/load-test/index?view=vsts)服務。</span><span class="sxs-lookup"><span data-stu-id="a8e15-125">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="a8e15-126">服務支援下列類型的測試格式：</span><span class="sxs-lookup"><span data-stu-id="a8e15-126">The service supports the following types of test format:</span></span>

- <span data-ttu-id="a8e15-127">Visual Studio 測試，– Visual Studio 中建立的 web 測試。</span><span class="sxs-lookup"><span data-stu-id="a8e15-127">Visual Studio test – web test created in Visual Studio.</span></span>
- <span data-ttu-id="a8e15-128">HTTP 封存式測試 – 封存內擷取的 HTTP 流量是在測試期間重新執行。</span><span class="sxs-lookup"><span data-stu-id="a8e15-128">HTTP Archive-based test – captured HTTP traffic inside archive is replayed during testing.</span></span>
- <span data-ttu-id="a8e15-129">[URL 為基礎的測試](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts)– 可讓您指定負載測試中，要求類型、 標頭和查詢字串的 Url。</span><span class="sxs-lookup"><span data-stu-id="a8e15-129">[URL-based test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="a8e15-130">執行設定的參數，例如持續時間，您可以設定負載模式、 使用者、 等等的數目。</span><span class="sxs-lookup"><span data-stu-id="a8e15-130">Run setting parameters such as duration, load pattern, number of users, etc., can be configured.</span></span>
- <span data-ttu-id="a8e15-131">[Apache JMeter](https://jmeter.apache.org/)測試。</span><span class="sxs-lookup"><span data-stu-id="a8e15-131">[Apache JMeter](https://jmeter.apache.org/) test.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="a8e15-132">Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="a8e15-132">Azure portal</span></span>

<span data-ttu-id="a8e15-133">[Azure 入口網站可讓設定及執行負載測試的 Web 應用程式，](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts)直接從 Azure 入口網站中的 App Service 中的 [效能] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="a8e15-133">[Azure portal allows setting up and running load testing of Web Apps,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the Performance tab of the App Service in Azure portal.</span></span>

![](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="a8e15-134">測試可以是具有指定的 URL 或 Visual Studio Web 測試檔案，其中可以測試多個 Url 的手動測試。</span><span class="sxs-lookup"><span data-stu-id="a8e15-134">The test can be a manual test with a specified URL, or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="a8e15-135">在測試結束時，會產生報表以顯示應用程式的效能特性。</span><span class="sxs-lookup"><span data-stu-id="a8e15-135">At end of the test, reports are generated to show the performance characteristics of the app.</span></span> <span data-ttu-id="a8e15-136">範例統計資料包括：</span><span class="sxs-lookup"><span data-stu-id="a8e15-136">Example statistics include:</span></span>

- <span data-ttu-id="a8e15-137">平均回應時間</span><span class="sxs-lookup"><span data-stu-id="a8e15-137">Average response time</span></span>
- <span data-ttu-id="a8e15-138">最大輸送量： 每秒要求數</span><span class="sxs-lookup"><span data-stu-id="a8e15-138">Max throughput: requests per second</span></span>
- <span data-ttu-id="a8e15-139">失敗百分比</span><span class="sxs-lookup"><span data-stu-id="a8e15-139">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="a8e15-140">第三方工具</span><span class="sxs-lookup"><span data-stu-id="a8e15-140">Third-party Tools</span></span>

<span data-ttu-id="a8e15-141">下列清單包含使用各種不同的功能集的第三方 web 效能工具：</span><span class="sxs-lookup"><span data-stu-id="a8e15-141">The following list contains third-party web performance tools with various feature sets:</span></span>

- <span data-ttu-id="a8e15-142">[Apache JMeter](https://jmeter.apache.org/) :負載測試工具的完整功能的套件。</span><span class="sxs-lookup"><span data-stu-id="a8e15-142">[Apache JMeter](https://jmeter.apache.org/) : Full featured suite of load testing tools.</span></span> <span data-ttu-id="a8e15-143">執行緒繫結： 需要每位使用者的一個執行緒。</span><span class="sxs-lookup"><span data-stu-id="a8e15-143">Thread-bound: need one thread per user.</span></span>
- [<span data-ttu-id="a8e15-144">ab-Apache HTTP 伺服器效能評定工具</span><span class="sxs-lookup"><span data-stu-id="a8e15-144">ab - Apache HTTP server benchmarking tool</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
- <span data-ttu-id="a8e15-145">[Gatling](https://gatling.io/) :使用 GUI 和測試的燒錄的桌面工具。</span><span class="sxs-lookup"><span data-stu-id="a8e15-145">[Gatling](https://gatling.io/) : Desktop tool with a GUI and test recorders.</span></span> <span data-ttu-id="a8e15-146">效能比 JMeter 的更好。</span><span class="sxs-lookup"><span data-stu-id="a8e15-146">More performant than JMeter.</span></span>
- <span data-ttu-id="a8e15-147">[Locust.io](https://locust.io/) :不受限制的執行緒。</span><span class="sxs-lookup"><span data-stu-id="a8e15-147">[Locust.io](https://locust.io/) : Not bounded by threads.</span></span>

<a name="add"></a>
## <a name="additional-resources"></a><span data-ttu-id="a8e15-148">其他資源</span><span class="sxs-lookup"><span data-stu-id="a8e15-148">Additional Resources</span></span>

<span data-ttu-id="a8e15-149">[載入測試部落格系列](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)由 Charlie sterling 主持。</span><span class="sxs-lookup"><span data-stu-id="a8e15-149">[Load Test blog series](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) by Charles Sterling.</span></span> <span data-ttu-id="a8e15-150">帶有但仍有相關之大部分主題。</span><span class="sxs-lookup"><span data-stu-id="a8e15-150">Dated but most of the topics are still relevant.</span></span>
