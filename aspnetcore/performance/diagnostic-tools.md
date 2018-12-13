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
# <a name="performance-diagnostic-tools"></a>效能診斷工具

藉由[Mike Rousos](https://github.com/mjrousos)

這篇文章列出對於診斷效能問題，ASP.NET Core 中的工具。

## <a name="visual-studio-diagnostic-tools"></a>Visual Studio 診斷工具

[分析與診斷工具](/visualstudio/profiling)建置到 Visual Studio 會啟動調查效能問題的好地方。 這些工具是功能強大且方便地從 Visual Studio 開發環境使用。 這項工具會允許 ASP.NET Core 應用程式中分析 CPU 使用量、 記憶體使用量和效能事件。

詳細資訊位於[Visual Studio 文件](/visualstudio/profiling/profiling-overview)。

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview)提供您的應用程式的深入的效能資料。 Application Insights 會自動收集回應率、 失敗率、 相依性回應時間，以及更多的資料。 Application Insights 支援您的應用程式記錄自訂事件和特定的計量。

Application Insights 可以用於各種環境中：

* 最佳化，可在 Azure 中運作。
* 在生產環境、 開發和預備環境中運作。
* 從在本機的運作方式[Visual Studio](/azure/application-insights/app-insights-visual-studio)或在其他裝載環境。

如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview)是一種效能分析工具，專為診斷.NET 效能問題的.NET 團隊所建立。 PerfView 可讓分析 CPU 使用量、 記憶體和 GC 的行為、 效能事件，以及時鐘時間。

您可以深入了解 PerfView，以及如何開始使用[PerfView 的影片教學課程](http://channel9.msdn.com/Series/PerfView-Tutorial)或閱讀工具中可用的使用者指南或[在 GitHub 上](https://github.com/Microsoft/perfview)。

## <a name="perfcollect"></a>PerfCollect

PerfView 是一種實用的效能分析工具.NET 案例，它只能在執行 Windows 讓您無法使用它來在 Linux 環境中執行的 ASP.NET Core 應用程式從收集追蹤。

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)是使用原生 Linux 程式碼剖析工具的 bash 指令碼 ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page)並[LTTng](https://lttng.org/)) 收集追蹤，可依 PerfView 進行分析的 Linux 上。 在其中 PerfView 不能直接使用的 Linux 環境中顯示的效能問題時，則 PerfCollect 會很有用。 相反地，PerfCollect 可以從收集追蹤，以便分析的.NET Core 應用程式在 Windows 電腦上使用 PerfView。

提供的詳細資訊如何安裝和開始使用 PerfCollect [GitHub 上](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)。
