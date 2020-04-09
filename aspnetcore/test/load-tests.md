---
title: ASP.NET核心負載/應力測試
author: Jeremy-Meng
description: 瞭解 ASP.NET Core 應用的負載測試和壓力測試的幾個值得注意的工具和方法。
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 1fd77a767fb53b9276081dd712e13108094a0382
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664686"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="871b4-103">ASP.NET核心負載/應力測試</span><span class="sxs-lookup"><span data-stu-id="871b4-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="871b4-104">負載測試和壓力測試對於確保 Web 應用具有可執行性和可擴充性非常重要。</span><span class="sxs-lookup"><span data-stu-id="871b4-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="871b4-105">他們的目標是不同的,即使他們經常分享類似的測試。</span><span class="sxs-lookup"><span data-stu-id="871b4-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="871b4-106">**負載測試**&ndash;測試應用是否可以處理特定方案指定的使用者負載,同時仍滿足回應目標。</span><span class="sxs-lookup"><span data-stu-id="871b4-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="871b4-107">該應用程式在正常情況下運行。</span><span class="sxs-lookup"><span data-stu-id="871b4-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="871b4-108">**壓力測試**&ndash;測試在極端條件下運行時測試應用的穩定性,通常長時間運行。</span><span class="sxs-lookup"><span data-stu-id="871b4-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="871b4-109">測試將高使用者負載(峰值或逐漸增加負載)放在應用上,或者限制應用的計算資源。</span><span class="sxs-lookup"><span data-stu-id="871b4-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="871b4-110">壓力測試確定受壓力的應用是否可以從故障中恢復,並優雅地返回到預期行為。</span><span class="sxs-lookup"><span data-stu-id="871b4-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="871b4-111">在壓力下,應用程式在正常情況下無法運行。</span><span class="sxs-lookup"><span data-stu-id="871b4-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="871b4-112">Visual Studio 2019 是具有負載測試功能的 Visual Studio 的最後一個版本。</span><span class="sxs-lookup"><span data-stu-id="871b4-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="871b4-113">對於將來需要負載測試工具的客戶,我們建議使用備用工具,如 Apache JMeter、Akamai 雲端測試和 BlazeMeter。</span><span class="sxs-lookup"><span data-stu-id="871b4-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="871b4-114">有關詳細資訊,請參閱 Visual [Studio 2019 發行說明](/visualstudio/releases/2019/release-notes-v16.0#test-tools)。</span><span class="sxs-lookup"><span data-stu-id="871b4-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="871b4-115">Visual Studio 工具</span><span class="sxs-lookup"><span data-stu-id="871b4-115">Visual Studio tools</span></span>

<span data-ttu-id="871b4-116">Visual Studio 允許用戶創建、開發和調試 Web 性能和負載測試。</span><span class="sxs-lookup"><span data-stu-id="871b4-116">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="871b4-117">可透過在 Web 瀏覽器中記錄操作來創建測試的選項。</span><span class="sxs-lookup"><span data-stu-id="871b4-117">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="871b4-118">有關如何使用 Visual Studio 2017 建立、設定和執行負載測試專案的資訊,請參閱[快速入門:建立負載測試專案](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)。</span><span class="sxs-lookup"><span data-stu-id="871b4-118">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="871b4-119">負載測試可以配置為在內部運行或使用 Azure DevOps 在雲端中執行。</span><span class="sxs-lookup"><span data-stu-id="871b4-119">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="871b4-120">第三方工具</span><span class="sxs-lookup"><span data-stu-id="871b4-120">Third-party tools</span></span>

<span data-ttu-id="871b4-121">以下清單包含具有各種功能集的第三方 Web 效能工具:</span><span class="sxs-lookup"><span data-stu-id="871b4-121">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="871b4-122">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="871b4-122">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="871b4-123">阿帕奇Bench(ab)</span><span class="sxs-lookup"><span data-stu-id="871b4-123">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="871b4-124">加特林</span><span class="sxs-lookup"><span data-stu-id="871b4-124">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="871b4-125">k6</span><span class="sxs-lookup"><span data-stu-id="871b4-125">k6</span></span>](https://k6.io)
* [<span data-ttu-id="871b4-126">蝗蟲</span><span class="sxs-lookup"><span data-stu-id="871b4-126">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="871b4-127">西風網浪</span><span class="sxs-lookup"><span data-stu-id="871b4-127">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="871b4-128">網林</span><span class="sxs-lookup"><span data-stu-id="871b4-128">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="871b4-129">韋加塔</span><span class="sxs-lookup"><span data-stu-id="871b4-129">Vegeta</span></span>](https://github.com/tsenart/vegeta)

