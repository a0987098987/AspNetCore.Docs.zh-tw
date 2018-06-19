---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: 了解動作篩選條件 (C#) |Microsoft 文件
author: microsoft
description: 本教學課程的目標是以說明動作篩選條件。 動作篩選條件是屬性，您可以套用至控制器動作--或整個控制器...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: d68933297329370e227f524c4b96ed7e259ef833
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870410"
---
<a name="understanding-action-filters-c"></a>了解動作篩選條件 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> 本教學課程的目標是以說明動作篩選條件。 動作篩選條件是您可以套用至控制器動作--或整個 controller--修改的方式執行動作的屬性。


## <a name="understanding-action-filters"></a>了解動作篩選條件

本教學課程的目標是以說明動作篩選條件。 動作篩選條件是您可以套用至控制器動作--或整個 controller--修改的方式執行動作的屬性。 ASP.NET MVC 架構包括數個動作篩選條件：

- OutputCache – 此動作篩選條件會快取指定的時間量的控制器動作的輸出。
- HandleError – 此動作篩選條件來處理控制器動作執行時引發的錯誤。
- 授權-本動作篩選條件可讓您限制存取特定使用者或角色。

您也可以建立您自己的自訂動作篩選條件。 例如，您可以建立自訂動作篩選條件，以實作自訂驗證系統。 或者，您可能想要建立動作篩選條件，以修改控制器動作所傳回的檢視資料。

在本教學課程中，您可以了解如何建置從頭動作篩選條件。 我們建立了記錄動作篩選條件的不同階段的處理動作記錄至 Visual Studio 輸出視窗。

### <a name="using-an-action-filter"></a>使用動作篩選條件

動作篩選條件是屬性。 您可以將大部分的動作篩選條件套用至個別控制器動作或整個控制器。

例如，資料中的控制站清單 1 會公開名為動作`Index()`，傳回目前的時間。 這個動作以裝飾`OutputCache`動作篩選條件。 此篩選條件會導致快取為 10 秒的動作所傳回的值。

**列出 1 – `Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

如果您重複叫用`Index()`動作將您的瀏覽器的網址列輸入 URL/資料/索引，並按 [重新整理] 按鈕多次，您將會看到相同的時間為 10 秒。 輸出`Index()`動作會快取 （請參閱圖 1） 的 10 秒。


[![快取的時間](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**圖 01**： 快取時間 ([按一下以檢視完整大小的影像](understanding-action-filters-cs/_static/image3.png))


在清單 1 中，單一動作篩選 –`OutputCache`動作篩選條件 – 套用至`Index()`方法。 如果您需要您可以套用多個動作篩選條件至相同的動作。 例如，您可能想要套用這兩`OutputCache`和`HandleError`動作篩選條件至相同的動作。

列出第 1`OutputCache`動作篩選條件套用至`Index()`動作。 您也可以將套用此屬性為`DataController`類別本身。 在此情況下，任何由控制器的動作所傳回的結果會快取 10 秒。

### <a name="the-different-types-of-filters"></a>不同類型的篩選器

ASP.NET MVC 架構支援四種不同類型的篩選：

1. 授權篩選 – 實作`IAuthorizationFilter`屬性。
2. 動作篩選 – 實作`IActionFilter`屬性。
3. 產生篩選 – 實作`IResultFilter`屬性。
4. 例外狀況篩選條件 – 實作`IExceptionFilter`屬性。

篩選條件的執行以上所列的順序。 例如，授權篩選條件一定會執行動作篩選條件之前，並篩選器的型別之後一律執行例外狀況篩選條件。

授權篩選條件可用於實作驗證和授權適用於控制器動作。 例如，授權篩選條件是授權篩選條件的範例。

動作篩選條件包含控制器動作執行前後執行的邏輯。 您可以修改控制器動作傳回的檢視資料，比方說，使用動作篩選條件。

結果篩選條件會包含執行之前和之後執行檢視結果的邏輯。 例如，您可能想要修改檢視結果 檢視呈現至瀏覽器之前，以滑鼠右鍵。

例外狀況篩選條件會篩選條件執行的最後一個類型。 您可以使用例外狀況篩選條件來處理您的控制器動作或控制器動作結果所引發的錯誤。 您也可以使用例外狀況篩選條件來記錄錯誤。

以特定順序執行各種不同類型的篩選器。 如果您想要控制的執行相同類型的篩選條件的順序，您可以設定篩選器順序屬性。

所有動作篩選條件的基底類別是`System.Web.Mvc.FilterAttribute`類別。 如果您想要實作特定類型的篩選條件，則您必須建立繼承自基底的篩選條件類別並實作一或多個類別`IAuthorizationFilter`， `IActionFilter`， `IResultFilter`，或`IExceptionFilter`介面。

### <a name="the-base-actionfilterattribute-class"></a>基底的 Actionfilterattribut 類別

為了讓您更輕鬆地實作自訂動作篩選條件，ASP.NET MVC 架構包括基底`ActionFilterAttribute`類別。 這個類別會同時實作`IActionFilter`和`IResultFilter`介面，並繼承自`Filter`類別。

無法完全一致的術語。 技術上來說，從 Actionfilterattribut 類別繼承的類別是動作篩選條件和結果篩選條件。 不過，鬆散的意義而言，文字的動作篩選條件用來參考任何一種 ASP.NET MVC 架構中的篩選器。

基底`ActionFilterAttribute`類別具有可以覆寫下列方法：

- OnActionExecuting – 執行控制器動作之前，會呼叫這個方法。
- OnActionExecuted – 控制器動作執行之後，會呼叫這個方法。
- OnResultExecuting – 控制器動作結果執行之前，會呼叫這個方法。
- OnResultExecuted – 控制器動作結果執行之後，會呼叫這個方法。

在下一步 區段中，我們會看到實作每個不同方法。

### <a name="creating-a-log-action-filter"></a>建立記錄動作篩選條件

若要說明如何建置自訂動作篩選條件，我們將建立自訂動作篩選條件的記錄檔的階段處理控制器動作，以 Visual Studio 輸出視窗。 我們`LogActionFilter`包含清單 2 中。

**列出 2 – `ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

在列出 2 中， `OnActionExecuting()`， `OnActionExecuted()`， `OnResultExecuting()`，和`OnResultExecuted()`所有方法都呼叫`Log()`方法。 方法的名稱和目前的路由資料傳遞至`Log()`方法。 `Log()`方法會將訊息寫入 Visual Studio 輸出 視窗 （請參閱圖 2）。


[![寫入至 Visual Studio [輸出] 視窗](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**圖 02**： 寫入至 Visual Studio [輸出] 視窗 ([按一下以檢視完整大小的影像](understanding-action-filters-cs/_static/image6.png))


Home 控制器中列出的 3 說明如何將記錄動作篩選條件套用至整個控制器類別。 每當任何由主控制器的動作會叫用 – 請`Index()`方法或`About()`方法-動作會記錄至 Visual Studio [輸出] 視窗的處理的階段。

**列出 3 – `Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>總結

在本教學課程中，您是導入 ASP.NET MVC 動作篩選條件。 您發現了四個不同類型的篩選： 授權篩選條件、 動作篩選條件、 結果篩選條件和例外狀況篩選條件。 您也學到基底`ActionFilterAttribute`類別。

最後，您學會如何實作簡單動作篩選條件。 我們建立記錄檔的階段處理 Visual Studio 輸出 視窗的控制器動作記錄動作篩選條件。

> [!div class="step-by-step"]
> [上一頁](asp-net-mvc-routing-overview-cs.md)
> [下一頁](improving-performance-with-output-caching-cs.md)
