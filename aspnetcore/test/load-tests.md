---
title: ASP.NET Core 負載/壓力測試
author: Jeremy-Meng
description: 瞭解 ASP.NET Core 應用程式的負載測試和壓力測試的幾項值得注意的工具和方法。
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: test/loadtests
ms.openlocfilehash: 5df2dd906d52aaec4fc13b07f3d92c87c802f37f
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406507"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="e0eb0-103">ASP.NET Core 負載/壓力測試</span><span class="sxs-lookup"><span data-stu-id="e0eb0-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="e0eb0-104">負載測試和壓力測試非常重要，可確保 web 應用程式的效能和可擴充性。</span><span class="sxs-lookup"><span data-stu-id="e0eb0-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="e0eb0-105">它們的目標會不同，即使它們經常共用類似的測試也一樣。</span><span class="sxs-lookup"><span data-stu-id="e0eb0-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="e0eb0-106">**負載測試**：測試應用程式是否可以在特定案例中處理指定的使用者負載，同時仍然滿足回應目標。</span><span class="sxs-lookup"><span data-stu-id="e0eb0-106">**Load tests**: Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="e0eb0-107">應用程式會在正常情況下執行。</span><span class="sxs-lookup"><span data-stu-id="e0eb0-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="e0eb0-108">**壓力測試**：在極端情況下執行時，測試應用程式穩定性通常長時間執行。</span><span class="sxs-lookup"><span data-stu-id="e0eb0-108">**Stress tests**: Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="e0eb0-109">測試會在應用程式上放置高使用者負載、尖峰或逐漸增加的負載，或限制應用程式的運算資源。</span><span class="sxs-lookup"><span data-stu-id="e0eb0-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="e0eb0-110">壓力測試會判斷壓力不足的應用程式是否可以從失敗中復原，並正常地回到預期的行為。</span><span class="sxs-lookup"><span data-stu-id="e0eb0-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="e0eb0-111">在壓力之下，應用程式不會在正常情況下執行。</span><span class="sxs-lookup"><span data-stu-id="e0eb0-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="e0eb0-112">Visual Studio 2019 宣佈[取代負載測試的](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/)計畫。</span><span class="sxs-lookup"><span data-stu-id="e0eb0-112">Visual Studio 2019 announced plans to [deprecate the load testing](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span> <span data-ttu-id="e0eb0-113">對應的 Azure DevOps 雲端式負載測試服務已關閉。</span><span class="sxs-lookup"><span data-stu-id="e0eb0-113">The corresponding Azure DevOps cloud-based load testing service has been closed.</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="e0eb0-114">協力廠商工具</span><span class="sxs-lookup"><span data-stu-id="e0eb0-114">Third-party tools</span></span>

<span data-ttu-id="e0eb0-115">下列清單包含具有各種功能集的協力廠商 web 效能工具：</span><span class="sxs-lookup"><span data-stu-id="e0eb0-115">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="e0eb0-116">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="e0eb0-116">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="e0eb0-117">ApacheBench （ab）</span><span class="sxs-lookup"><span data-stu-id="e0eb0-117">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="e0eb0-118">Gatling</span><span class="sxs-lookup"><span data-stu-id="e0eb0-118">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="e0eb0-119">k6</span><span class="sxs-lookup"><span data-stu-id="e0eb0-119">k6</span></span>](https://k6.io)
* [<span data-ttu-id="e0eb0-120">Locust</span><span class="sxs-lookup"><span data-stu-id="e0eb0-120">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="e0eb0-121">West 風 WebSurge</span><span class="sxs-lookup"><span data-stu-id="e0eb0-121">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="e0eb0-122">Netling</span><span class="sxs-lookup"><span data-stu-id="e0eb0-122">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="e0eb0-123">Vegeta</span><span class="sxs-lookup"><span data-stu-id="e0eb0-123">Vegeta</span></span>](https://github.com/tsenart/vegeta)