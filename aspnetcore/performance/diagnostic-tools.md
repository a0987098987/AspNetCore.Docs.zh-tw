---
title: 效能診斷工具
author: mjrousos
description: 適用於診斷 ASP.NET Core 應用程式中的效能問題的有用工具。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329195"
---
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="517c9-103">效能診斷工具</span><span class="sxs-lookup"><span data-stu-id="517c9-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="517c9-104">藉由[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="517c9-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="517c9-105">這篇文章列出對於診斷效能問題，ASP.NET Core 中的工具。</span><span class="sxs-lookup"><span data-stu-id="517c9-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="517c9-106">Visual Studio 診斷工具</span><span class="sxs-lookup"><span data-stu-id="517c9-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="517c9-107">[分析與診斷工具](/visualstudio/profiling)建置到 Visual Studio 會啟動調查效能問題的好地方。</span><span class="sxs-lookup"><span data-stu-id="517c9-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="517c9-108">這些工具是功能強大且方便地從 Visual Studio 開發環境使用。</span><span class="sxs-lookup"><span data-stu-id="517c9-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="517c9-109">這項工具會允許 ASP.NET Core 應用程式中分析 CPU 使用量、 記憶體使用量和效能事件。</span><span class="sxs-lookup"><span data-stu-id="517c9-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span>

<span data-ttu-id="517c9-110">詳細資訊位於[Visual Studio 文件](/visualstudio/profiling/profiling-overview)。</span><span class="sxs-lookup"><span data-stu-id="517c9-110">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="517c9-111">Application Insights</span><span class="sxs-lookup"><span data-stu-id="517c9-111">Application Insights</span></span>

<span data-ttu-id="517c9-112">[Application Insights](/azure/application-insights/app-insights-overview)提供您的應用程式的深入的效能資料。</span><span class="sxs-lookup"><span data-stu-id="517c9-112">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="517c9-113">Application Insights 會自動收集回應率、 失敗率、 相依性回應時間，以及更多的資料。</span><span class="sxs-lookup"><span data-stu-id="517c9-113">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="517c9-114">Application Insights 支援您的應用程式記錄自訂事件和特定的計量。</span><span class="sxs-lookup"><span data-stu-id="517c9-114">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="517c9-115">Application Insights 可以用於各種環境中：</span><span class="sxs-lookup"><span data-stu-id="517c9-115">Application Insights can be used in a variety environments:</span></span>

* <span data-ttu-id="517c9-116">最佳化，可在 Azure 中運作。</span><span class="sxs-lookup"><span data-stu-id="517c9-116">Optimized to work in Azure.</span></span>
* <span data-ttu-id="517c9-117">在生產環境、 開發和預備環境中運作。</span><span class="sxs-lookup"><span data-stu-id="517c9-117">Works in production, development, and staging.</span></span>
* <span data-ttu-id="517c9-118">從在本機的運作方式[Visual Studio](/azure/application-insights/app-insights-visual-studio)或在其他裝載環境。</span><span class="sxs-lookup"><span data-stu-id="517c9-118">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="517c9-119">如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="517c9-119">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="517c9-120">PerfView</span><span class="sxs-lookup"><span data-stu-id="517c9-120">PerfView</span></span>

<span data-ttu-id="517c9-121">[PerfView](https://github.com/Microsoft/perfview)是一種效能分析工具，專為診斷.NET 效能問題的.NET 團隊所建立。</span><span class="sxs-lookup"><span data-stu-id="517c9-121">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="517c9-122">PerfView 可讓分析 CPU 使用量、 記憶體和 GC 的行為、 效能事件，以及時鐘時間。</span><span class="sxs-lookup"><span data-stu-id="517c9-122">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="517c9-123">您可以深入了解 PerfView，以及如何開始使用[PerfView 的影片教學課程](http://channel9.msdn.com/Series/PerfView-Tutorial)或閱讀工具中可用的使用者指南或[在 GitHub 上](https://github.com/Microsoft/perfview)。</span><span class="sxs-lookup"><span data-stu-id="517c9-123">You can learn more about PerfView and how to get started with [PerfView video tutorials](http://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="perfcollect"></a><span data-ttu-id="517c9-124">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="517c9-124">PerfCollect</span></span>

<span data-ttu-id="517c9-125">PerfView 是一種實用的效能分析工具.NET 案例，它只能在執行 Windows 讓您無法使用它來在 Linux 環境中執行的 ASP.NET Core 應用程式從收集追蹤。</span><span class="sxs-lookup"><span data-stu-id="517c9-125">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="517c9-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)是使用原生 Linux 程式碼剖析工具的 bash 指令碼 ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page)並[LTTng](https://lttng.org/)) 收集追蹤，可依 PerfView 進行分析的 Linux 上。</span><span class="sxs-lookup"><span data-stu-id="517c9-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="517c9-127">在其中 PerfView 不能直接使用的 Linux 環境中顯示的效能問題時，則 PerfCollect 會很有用。</span><span class="sxs-lookup"><span data-stu-id="517c9-127">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="517c9-128">相反地，PerfCollect 可以從收集追蹤，以便分析的.NET Core 應用程式在 Windows 電腦上使用 PerfView。</span><span class="sxs-lookup"><span data-stu-id="517c9-128">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="517c9-129">提供的詳細資訊如何安裝和開始使用 PerfCollect [GitHub 上](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)。</span><span class="sxs-lookup"><span data-stu-id="517c9-129">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>
