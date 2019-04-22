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
# <a name="performance-diagnostic-tools"></a>效能診斷工具

藉由[Mike Rousos](https://github.com/mjrousos)

這篇文章列出對於診斷效能問題，ASP.NET Core 中的工具。

## <a name="visual-studio-diagnostic-tools"></a>Visual Studio 診斷工具

[分析與診斷工具](/visualstudio/profiling)建置到 Visual Studio 會啟動調查效能問題的好地方。 這些工具是功能強大且方便地從 Visual Studio 開發環境使用。 這項工具會允許 ASP.NET Core 應用程式中分析 CPU 使用量、 記憶體使用量和效能事件。 正在內建可讓您在開發階段的簡單程式碼剖析。

詳細資訊位於[Visual Studio 文件](/visualstudio/profiling/profiling-overview)。

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview)提供您的應用程式的深入的效能資料。 Application Insights 會自動收集回應率、 失敗率、 相依性回應時間，以及更多的資料。 Application Insights 支援您的應用程式記錄自訂事件和特定的計量。

Azure Application Insights 提供多種方法來提供受監視的應用程式的深入資訊：

- [應用程式對應](/azure/application-insights/app-insights-app-map)– 所有元件的分散式應用程式，幫助找出效能瓶頸或熱點失敗-點。
- [Azure 計量瀏覽器](/azure/azure-monitor/platform/metrics-getting-started)是可讓您繪製圖表，以視覺方式串連趨勢，Microsoft Azure 入口網站的元件，並調查尖峰和下降中計量的值。
- [在 Application Insights 入口網站中的 [效能] 刀鋒視窗](/azure/application-insights/app-insights-tutorial-performance):

  - 顯示受監視的應用程式中的不同作業的效能詳細資料。
  - 允許鑽研至單一作業若要檢查的所有組件/相依性持續時間較長的參與。
  - Profiler 可以從這裡，以收集效能追蹤隨叫用。

- [Azure Application Insights Profiler](/azure/azure-monitor/app/profiler)允許規則，以及視分析的.NET 應用程式。  Azure 入口網站會顯示擷取效能追蹤呼叫堆疊和忙碌路徑。 追蹤檔案也可以下載以便使用 PerfView 的深入分析。

Application Insights 可以用於各種環境中：

- 最佳化，可在 Azure 中運作。
- 在生產環境、 開發和預備環境中運作。
- 從在本機的運作方式[Visual Studio](/azure/application-insights/app-insights-visual-studio)或在其他裝載環境。

如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview)是一種效能分析工具，專為診斷.NET 效能問題的.NET 團隊所建立。 PerfView 可讓分析 CPU 使用量、 記憶體和 GC 的行為、 效能事件，以及時鐘時間。

您可以深入了解 PerfView，以及如何開始使用[PerfView 的影片教學課程](http://channel9.msdn.com/Series/PerfView-Tutorial)或閱讀工具中可用的使用者指南或[在 GitHub 上](https://github.com/Microsoft/perfview)。

## <a name="windows-performance-toolkit"></a>Windows 效能工具組

[Windows 效能工具組](/windows-hardware/test/wpt/)(WPT) 是由兩個元件所組成：Windows Performance Recorder (WPR) 和 Windows Performance Analyzer (WPA)。 工具產生 Windows 作業系統和應用程式的深入效能設定的檔。 WPT 有更豐富的方式視覺化資料，但其收集的資料比 PerfView 的權力較小。

## <a name="perfcollect"></a>PerfCollect

PerfView 是一種實用的效能分析工具.NET 案例，它只能在執行 Windows 讓您無法使用它來在 Linux 環境中執行的 ASP.NET Core 應用程式從收集追蹤。

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)是使用原生 Linux 程式碼剖析工具的 bash 指令碼 ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page)並[LTTng](https://lttng.org/)) 收集追蹤，可依 PerfView 進行分析的 Linux 上。 在其中 PerfView 不能直接使用的 Linux 環境中顯示的效能問題時，則 PerfCollect 會很有用。 相反地，PerfCollect 可以從收集追蹤，以便分析的.NET Core 應用程式在 Windows 電腦上使用 PerfView。

提供的詳細資訊如何安裝和開始使用 PerfCollect [GitHub 上](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)。

## <a name="other-third-party-performance-tools"></a>其他協力廠商效能工具

以下列出一些可用於效能調查的.NET Core 應用程式的協力廠商效能工具。

- [MiniProfiler](https://miniprofiler.com/)
- dotTrace 和 jetbrains dotMemory
- 從 Intel VTune
