---
title: 效能診斷工具
author: mjrousos
description: 適用於診斷 ASP.NET Core 應用程式中的效能問題的有用工具。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/diagnostic-tools
ms.openlocfilehash: 66676b5a2b95b87bfbbd50022e279e35a12b9793
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59516218"
---
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="9c612-103">效能診斷工具</span><span class="sxs-lookup"><span data-stu-id="9c612-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="9c612-104">藉由[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="9c612-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="9c612-105">這篇文章列出對於診斷效能問題，ASP.NET Core 中的工具。</span><span class="sxs-lookup"><span data-stu-id="9c612-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="9c612-106">Visual Studio 診斷工具</span><span class="sxs-lookup"><span data-stu-id="9c612-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="9c612-107">[分析與診斷工具](/visualstudio/profiling)建置到 Visual Studio 會啟動調查效能問題的好地方。</span><span class="sxs-lookup"><span data-stu-id="9c612-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="9c612-108">這些工具是功能強大且方便地從 Visual Studio 開發環境使用。</span><span class="sxs-lookup"><span data-stu-id="9c612-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="9c612-109">這項工具會允許 ASP.NET Core 應用程式中分析 CPU 使用量、 記憶體使用量和效能事件。</span><span class="sxs-lookup"><span data-stu-id="9c612-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span> <span data-ttu-id="9c612-110">正在內建可讓您在開發階段的簡單程式碼剖析。</span><span class="sxs-lookup"><span data-stu-id="9c612-110">Being built-in makes profiling easy at development time.</span></span>

<span data-ttu-id="9c612-111">詳細資訊位於[Visual Studio 文件](/visualstudio/profiling/profiling-overview)。</span><span class="sxs-lookup"><span data-stu-id="9c612-111">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="9c612-112">Application Insights</span><span class="sxs-lookup"><span data-stu-id="9c612-112">Application Insights</span></span>

<span data-ttu-id="9c612-113">[Application Insights](/azure/application-insights/app-insights-overview)提供您的應用程式的深入的效能資料。</span><span class="sxs-lookup"><span data-stu-id="9c612-113">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="9c612-114">Application Insights 會自動收集回應率、 失敗率、 相依性回應時間，以及更多的資料。</span><span class="sxs-lookup"><span data-stu-id="9c612-114">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="9c612-115">Application Insights 支援您的應用程式記錄自訂事件和特定的計量。</span><span class="sxs-lookup"><span data-stu-id="9c612-115">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="9c612-116">Azure Application Insights 提供多種方法來提供受監視的應用程式的深入資訊：</span><span class="sxs-lookup"><span data-stu-id="9c612-116">Azure Application Insights provides multiple ways to give insights on monitored apps:</span></span>

- <span data-ttu-id="9c612-117">[應用程式對應](/azure/application-insights/app-insights-app-map)– 所有元件的分散式應用程式，幫助找出效能瓶頸或熱點失敗-點。</span><span class="sxs-lookup"><span data-stu-id="9c612-117">[Application Map](/azure/application-insights/app-insights-app-map) – helps spot performance bottlenecks or failure hot-spots across all components of distributed apps.</span></span>
- <span data-ttu-id="9c612-118">[Azure 計量瀏覽器](/azure/azure-monitor/platform/metrics-getting-started)是可讓您繪製圖表，以視覺方式串連趨勢，Microsoft Azure 入口網站的元件，並調查尖峰和下降中計量的值。</span><span class="sxs-lookup"><span data-stu-id="9c612-118">[Azure Metrics Explorer](/azure/azure-monitor/platform/metrics-getting-started) is a component of the Microsoft Azure portal that allows plotting charts, visually correlating trends, and investigating spikes and dips in metrics' values.</span></span>
- <span data-ttu-id="9c612-119">[在 Application Insights 入口網站中的 [效能] 刀鋒視窗](/azure/application-insights/app-insights-tutorial-performance):</span><span class="sxs-lookup"><span data-stu-id="9c612-119">[Performance blade in Application Insights portal](/azure/application-insights/app-insights-tutorial-performance):</span></span>

  - <span data-ttu-id="9c612-120">顯示受監視的應用程式中的不同作業的效能詳細資料。</span><span class="sxs-lookup"><span data-stu-id="9c612-120">Shows performance details for different operations in the monitored app.</span></span>
  - <span data-ttu-id="9c612-121">允許鑽研至單一作業若要檢查的所有組件/相依性持續時間較長的參與。</span><span class="sxs-lookup"><span data-stu-id="9c612-121">Allows drilling into a single operation to check all parts/dependencies that contribute to a long duration.</span></span>
  - <span data-ttu-id="9c612-122">Profiler 可以從這裡，以收集效能追蹤隨叫用。</span><span class="sxs-lookup"><span data-stu-id="9c612-122">Profiler can be invoked from here to collect performance traces on-demand.</span></span>

- <span data-ttu-id="9c612-123">[Azure Application Insights Profiler](/azure/azure-monitor/app/profiler)允許規則，以及視分析的.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9c612-123">[Azure Application Insights Profiler](/azure/azure-monitor/app/profiler) allows regular and on-demand profiling of .NET apps.</span></span>  <span data-ttu-id="9c612-124">Azure 入口網站會顯示擷取效能追蹤呼叫堆疊和忙碌路徑。</span><span class="sxs-lookup"><span data-stu-id="9c612-124">Azure portal shows captured performance traces with call stacks and hot paths.</span></span> <span data-ttu-id="9c612-125">追蹤檔案也可以下載以便使用 PerfView 的深入分析。</span><span class="sxs-lookup"><span data-stu-id="9c612-125">The trace files can also be downloaded for deeper analysis using PerfView.</span></span>

<span data-ttu-id="9c612-126">Application Insights 可以用於各種環境中：</span><span class="sxs-lookup"><span data-stu-id="9c612-126">Application Insights can be used in a variety environments:</span></span>

- <span data-ttu-id="9c612-127">最佳化，可在 Azure 中運作。</span><span class="sxs-lookup"><span data-stu-id="9c612-127">Optimized to work in Azure.</span></span>
- <span data-ttu-id="9c612-128">在生產環境、 開發和預備環境中運作。</span><span class="sxs-lookup"><span data-stu-id="9c612-128">Works in production, development, and staging.</span></span>
- <span data-ttu-id="9c612-129">從在本機的運作方式[Visual Studio](/azure/application-insights/app-insights-visual-studio)或在其他裝載環境。</span><span class="sxs-lookup"><span data-stu-id="9c612-129">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="9c612-130">如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="9c612-130">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="9c612-131">PerfView</span><span class="sxs-lookup"><span data-stu-id="9c612-131">PerfView</span></span>

<span data-ttu-id="9c612-132">[PerfView](https://github.com/Microsoft/perfview)是一種效能分析工具，專為診斷.NET 效能問題的.NET 團隊所建立。</span><span class="sxs-lookup"><span data-stu-id="9c612-132">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="9c612-133">PerfView 可讓分析 CPU 使用量、 記憶體和 GC 的行為、 效能事件，以及時鐘時間。</span><span class="sxs-lookup"><span data-stu-id="9c612-133">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="9c612-134">您可以深入了解 PerfView，以及如何開始使用[PerfView 的影片教學課程](http://channel9.msdn.com/Series/PerfView-Tutorial)或閱讀工具中可用的使用者指南或[在 GitHub 上](https://github.com/Microsoft/perfview)。</span><span class="sxs-lookup"><span data-stu-id="9c612-134">You can learn more about PerfView and how to get started with [PerfView video tutorials](http://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="windows-performance-toolkit"></a><span data-ttu-id="9c612-135">Windows 效能工具組</span><span class="sxs-lookup"><span data-stu-id="9c612-135">Windows Performance Toolkit</span></span>

<span data-ttu-id="9c612-136">[Windows 效能工具組](/windows-hardware/test/wpt/)(WPT) 是由兩個元件所組成：Windows Performance Recorder (WPR) 和 Windows Performance Analyzer (WPA)。</span><span class="sxs-lookup"><span data-stu-id="9c612-136">[Windows Performance Toolkit](/windows-hardware/test/wpt/) (WPT) consists of two components: Windows Performance Recorder (WPR) and Windows Performance Analyzer (WPA).</span></span> <span data-ttu-id="9c612-137">工具產生 Windows 作業系統和應用程式的深入效能設定的檔。</span><span class="sxs-lookup"><span data-stu-id="9c612-137">The tools produce in-depth performance profiles of Windows operating systems and apps.</span></span> <span data-ttu-id="9c612-138">WPT 有更豐富的方式視覺化資料，但其收集的資料比 PerfView 的權力較小。</span><span class="sxs-lookup"><span data-stu-id="9c612-138">WPT has richer ways of visualizing data, but its data collecting is less powerful than PerfView's.</span></span>

## <a name="perfcollect"></a><span data-ttu-id="9c612-139">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="9c612-139">PerfCollect</span></span>

<span data-ttu-id="9c612-140">PerfView 是一種實用的效能分析工具.NET 案例，它只能在執行 Windows 讓您無法使用它來在 Linux 環境中執行的 ASP.NET Core 應用程式從收集追蹤。</span><span class="sxs-lookup"><span data-stu-id="9c612-140">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="9c612-141">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)是使用原生 Linux 程式碼剖析工具的 bash 指令碼 ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page)並[LTTng](https://lttng.org/)) 收集追蹤，可依 PerfView 進行分析的 Linux 上。</span><span class="sxs-lookup"><span data-stu-id="9c612-141">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="9c612-142">在其中 PerfView 不能直接使用的 Linux 環境中顯示的效能問題時，則 PerfCollect 會很有用。</span><span class="sxs-lookup"><span data-stu-id="9c612-142">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="9c612-143">相反地，PerfCollect 可以從收集追蹤，以便分析的.NET Core 應用程式在 Windows 電腦上使用 PerfView。</span><span class="sxs-lookup"><span data-stu-id="9c612-143">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="9c612-144">提供的詳細資訊如何安裝和開始使用 PerfCollect [GitHub 上](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)。</span><span class="sxs-lookup"><span data-stu-id="9c612-144">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>

## <a name="other-third-party-performance-tools"></a><span data-ttu-id="9c612-145">其他協力廠商效能工具</span><span class="sxs-lookup"><span data-stu-id="9c612-145">Other Third-party Performance Tools</span></span>

<span data-ttu-id="9c612-146">以下列出一些可用於效能調查的.NET Core 應用程式的協力廠商效能工具。</span><span class="sxs-lookup"><span data-stu-id="9c612-146">The following lists some third-party performance tools that are useful in performance investigation of .NET Core applications.</span></span>

- [<span data-ttu-id="9c612-147">MiniProfiler</span><span class="sxs-lookup"><span data-stu-id="9c612-147">MiniProfiler</span></span>](https://miniprofiler.com/)
- <span data-ttu-id="9c612-148">dotTrace 和 jetbrains dotMemory</span><span class="sxs-lookup"><span data-stu-id="9c612-148">dotTrace and dotMemory from JetBrains</span></span>
- <span data-ttu-id="9c612-149">從 Intel VTune</span><span class="sxs-lookup"><span data-stu-id="9c612-149">VTune from Intel</span></span>
