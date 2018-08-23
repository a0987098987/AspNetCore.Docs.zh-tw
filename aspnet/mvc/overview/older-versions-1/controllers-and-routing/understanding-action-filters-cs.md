---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: 了解動作篩選條件 (C#) |Microsoft Docs
author: microsoft
description: 本教學課程的目標在於說明動作篩選條件。 動作篩選條件是屬性，您可以套用至控制器動作或整個控制器...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c7706d8252d5a0271f1b9243fa8eb282f722654
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832554"
---
<a name="understanding-action-filters-c"></a>了解動作篩選條件 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> 本教學課程的目標在於說明動作篩選條件。 動作篩選條件是屬性，您可以套用至控制器動作或整個 controller--修改 執行動作的方式。


## <a name="understanding-action-filters"></a>了解動作篩選條件

本教學課程的目標在於說明動作篩選條件。 動作篩選條件是屬性，您可以套用至控制器動作或整個 controller--修改 執行動作的方式。 ASP.NET MVC 架構包括數個動作篩選條件：

- OutputCache – 此動作篩選條件會快取一段指定時間的控制器動作的輸出。
- HandleError – 此動作篩選條件處理控制器動作執行時引發的錯誤。
- 授權-此動作篩選條件可讓您限制存取特定使用者或角色。

您也可以建立您自己的自訂動作篩選條件。 例如，您可能要建立自訂動作篩選條件，以實作自訂驗證系統。 或者，您可能想要建立動作篩選條件，以修改控制器動作所傳回的檢視資料。

在本教學課程中，您會學習如何建立動作篩選條件所打造的。 我們會建立記錄至 Visual Studio [輸出] 視窗的不同階段的處理程序的動作記錄動作篩選條件。

### <a name="using-an-action-filter"></a>使用動作篩選條件

動作篩選條件是屬性。 您可以將大部分的動作篩選條件套用至個別的控制器動作或整個控制器。

比方說，在 列表 1 中的資料控制者會公開名為動作`Index()`，傳回目前的時間。 此動作以裝飾`OutputCache`動作篩選條件。 此篩選條件會導致快取為 10 秒的動作所傳回的值。

**列表 1 – `Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

如果您重複叫用`Index()`由您的瀏覽器的網址列中輸入 URL/資料/索引，然後按 重新整理動作按鈕多次，，然後您會看到相同的時間為 10 秒。 輸出`Index()`動作的快取 （請參閱 圖 1） 的 10 秒。


[![快取的時間](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**圖 01**： 快取時間 ([按一下以檢視完整大小的影像](understanding-action-filters-cs/_static/image3.png))


在列表 1 中，單一動作篩選條件 –`OutputCache`套用至動作篩選條件 –`Index()`方法。 如果您需要您可以套用多個動作篩選條件至相同的動作。 例如，您可能想要套用這兩`OutputCache`和`HandleError`動作篩選條件至相同的動作。

在 [列表 1]`OutputCache`動作篩選條件會套用至`Index()`動作。 您也可以套用至這個屬性`DataController`類別本身。 在此情況下，任何由控制器的動作所傳回的結果會快取 10 秒的時間。

### <a name="the-different-types-of-filters"></a>不同類型的篩選器

ASP.NET MVC 架構支援四種不同類型的篩選：

1. 授權篩選條件 – 實作`IAuthorizationFilter`屬性。
2. 動作篩選條件 – 實作`IActionFilter`屬性。
3. 結果篩選條件 – 實作`IResultFilter`屬性。
4. 例外狀況篩選條件 – 實作`IExceptionFilter`屬性。

篩選條件的執行上面所列的順序。 例如，授權篩選條件永遠會在動作篩選條件之前執行，而例外狀況篩選條件一律會執行篩選器的型別之後。

授權篩選條件會用來實作驗證和授權適用於控制器動作。 比方說，授權篩選條件是為授權篩選條件的範例。

動作篩選條件包含之前和之後的控制器動作執行時，才會執行的邏輯。 您可以修改控制器動作傳回檢視資料，比方說，使用動作篩選條件。

結果篩選條件包含之前和之後執行檢視結果時，才會執行的邏輯。 例如，您可能想要修改檢視結果 檢視呈現至瀏覽器之前，以滑鼠右鍵。

例外狀況篩選條件會篩選條件執行的最後一個類型。 您可以使用例外狀況篩選條件來處理您的控制器動作或控制器動作結果所引發的錯誤。 您也可以使用例外狀況篩選條件，來記錄錯誤。

各種不同類型的篩選器會以特定順序執行。 如果您想要控制在其中執行相同類型的篩選條件的順序，您可以設定篩選條件的順序屬性。

所有動作篩選條件的基底類別是`System.Web.Mvc.FilterAttribute`類別。 如果您想要實作特定類型的篩選條件，則您需要建立一個類別，繼承自基底的篩選條件類別並實作一或多個`IAuthorizationFilter`， `IActionFilter`， `IResultFilter`，或`IExceptionFilter`介面。

### <a name="the-base-actionfilterattribute-class"></a>基底的 Actionfilterattribut 類別

為了讓您更輕鬆地實作自訂動作篩選條件，ASP.NET MVC 架構包括基底`ActionFilterAttribute`類別。 這個類別會實作`IActionFilter`並`IResultFilter`介面，並繼承自`Filter`類別。

無法完全一致的術語。 技術上來說，從 Actionfilterattribut 類別繼承的類別是動作篩選條件和結果篩選條件。 不過，鬆散的意義而言，文字的動作篩選條件用來參考任何類型的 ASP.NET MVC 架構中的篩選條件。

基底`ActionFilterAttribute`類別具有可以覆寫下列方法：

- OnActionExecuting – 執行控制器動作之前，會呼叫這個方法。
- OnActionExecuted – 控制器動作執行之後，會呼叫這個方法。
- OnResultExecuting – 控制器動作結果執行之前，會呼叫這個方法。
- OnResultExecuted – 控制器動作結果執行之後，會呼叫這個方法。

在下一步 區段中，我們會看到如何實作每一種不同的方法。

### <a name="creating-a-log-action-filter"></a>建立記錄的動作篩選條件

若要說明如何建置自訂動作篩選條件，我們將建立自訂動作篩選條件的記錄處理 Visual Studio 輸出 視窗的控制器動作的階段。 我們`LogActionFilter`列表 2 中包含。

**列表 2 – `ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

在列表 2 中， `OnActionExecuting()`， `OnActionExecuted()`， `OnResultExecuting()`，以及`OnResultExecuted()`方法的所有呼叫`Log()`方法。 方法的名稱和目前的路由資料傳遞至`Log()`方法。 `Log()`方法會將訊息寫入 Visual Studio 輸出 視窗 （請參閱 圖 2）。


[![寫入至 Visual Studio [輸出] 視窗](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**圖 02**： 寫入至 Visual Studio [輸出] 視窗 ([按一下以檢視完整大小的影像](understanding-action-filters-cs/_static/image6.png))


在 列表 3 中的主控制器會說明如何將記錄的動作篩選條件套用至整個控制器類別。 每當任何由主控制器的動作會叫用 – 請`Index()`方法或`About()`方法 – 動作都會記錄到 Visual Studio [輸出] 視窗的處理階段。

**列表 3 – `Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>總結

在本教學課程中，已向您介紹 ASP.NET MVC 動作篩選條件。 您已了解四種不同類型的篩選： 授權篩選條件、 動作篩選條件、 結果篩選條件和例外狀況篩選條件。 您也學會了基底`ActionFilterAttribute`類別。

最後，您已了解如何實作簡單動作篩選條件。 我們會建立記錄檔的處理 Visual Studio 輸出 視窗的控制器動作階段記錄動作篩選條件。

> [!div class="step-by-step"]
> [上一頁](asp-net-mvc-routing-overview-cs.md)
> [下一頁](improving-performance-with-output-caching-cs.md)
