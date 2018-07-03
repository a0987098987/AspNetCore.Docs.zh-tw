---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: 新功能 ASP.NET Web Pages 3.2 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: eb5698b2928c1f2d1016ec74c18a63e2df5b3225
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390429"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>什麼是 ASP.NET Web Pages 3.2 的新功能
====================
by [Microsoft](https://github.com/microsoft)

本主題會描述什麼是新的 ASP.NET Web Pages 3.2，網頁 3.2.2 和[網頁 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2

此版本修正的 bug，並導入了一項新功能。

## <a name="download"></a>下載

執行階段功能會以 NuGet 套件，NuGet gallery 上發行。 所有執行階段套件遵循[語意版本設定](http://semver.org/)規格。 ASP.NET Web Pages 3.2 套件有下列版本： &ldquo;3.2.0&rdquo;。 您可以安裝或更新這些套件，透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/)。 此版也包含在 NuGet 上的對應當地語系化的套件。

您可以安裝或使用 NuGet 套件管理員主控台更新到發行的 NuGet 套件：

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>新功能和 Bug 修正

我們修正某個錯誤，並在此版本中所做一個次要功能增強功能。 您可以尋找查詢的相同[此處](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)。

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

此版本中彙總變更[ASP.NET Web Pages 3.2.1 Beta 版](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)提供顯著的效能改善呈現大型的 razor 頁面。 請參閱[Codeplex 問題 585](https://aspnetwebstack.codeplex.com/workitem/585)。 此版本與 MVC 5.2.2 現在將會相依於此版本的套件。

我們與 MSN 小組合作在呈現大型分頁。 當頁面呈現超過 80 Kb 的資料時，我們會得到與大型物件堆積上的物件。 使用多個圖層的版面配置時就可以乘以此效果。

在伺服器上的結果是額外的 CPU 使用量、 較長的保留的記憶體，並更長期間會暫停[Gen 2 清除](https://msdn.microsoft.com/en-us/library/ms973837.aspx)在記憶體回收行程。

以下是示範的分析結果的資料表[perfview](https://channel9.msdn.com/Series/PerfView-Tutorial)執行。 CPU 保留常數在約 68%，而呈現大型分頁。 下表顯示的層代 2 集合數幾乎完全排除，其結果更高要求率和大量減少因為記憶體回收而暫停。

| **區域** | **Before (3.2)** | **之後 (3.2.1)** | **差異 %** |
| --- | --- | --- | --- |
| 要求總數 （計數） | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| 追蹤持續時間 （秒） | 196.20 | 198.60 | 1.20% |
| 要求/秒 | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| CPU 負載 | 68.80% | 68.50% |  -0.40% |
| GC CPU 範例 | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| 總配置 （計數） | 55,357,146 | 57,222,949 | 3.40% |
| 總 GC 暫停 （範例） | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC （計數） | 403 | 1,216 | 201.70% |
| Gen1 GC （計數） | 290 | 367 | 26.60% |
| Gen2 GC （計數） | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / 要求 （樣本/要求） | 19.73 | 16.47 | -16.50% |

| 色彩編碼： | <font style="background-color: #00ff00">Core 改進</font> | <font style="background-color: #4bacc6">對效能有正面的影響</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET 網頁 3.2.3 beta1

此版本包含只 bug 修正。 您可以使用[這項查詢](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修正的問題清單。
