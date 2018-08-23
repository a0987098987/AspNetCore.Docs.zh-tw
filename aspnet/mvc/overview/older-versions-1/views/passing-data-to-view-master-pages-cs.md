---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: 將資料傳遞至檢視主版頁面 (C#) |Microsoft Docs
author: microsoft
description: 本教學課程的目標是要說明如何，您可以從控制器傳遞資料至檢視主版頁面。 我們將探討兩種策略，將資料傳遞至檢視 m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: e04a9b274b735af05a8e08dc7d8f34f0d83605be
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833835"
---
<a name="passing-data-to-view-master-pages-c"></a>將資料傳遞至檢視主版頁面 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> 本教學課程的目標是要說明如何，您可以從控制器傳遞資料至檢視主版頁面。 我們將探討兩種策略，將資料傳遞至檢視主版頁面。 首先，我們會討論簡單的解決方案所產生的應用程式，很難維護。 接下來，我們會檢查較理想的方案需要稍微更多的初始工作，但更容易維護的應用程式中的結果。


## <a name="passing-data-to-view-master-pages"></a>將資料傳遞至檢視主版頁面

本教學課程的目標是要說明如何，您可以從控制器傳遞資料至檢視主版頁面。 我們將探討兩種策略，將資料傳遞至檢視主版頁面。 首先，我們會討論簡單的解決方案所產生的應用程式，很難維護。 接下來，我們會檢查較理想的方案需要稍微更多的初始工作，但更容易維護的應用程式中的結果。

### <a name="the-problem"></a>問題

假設您要建置影片資料庫應用程式，而您想要在您的應用程式中的每個頁面上顯示電影類別目錄的清單 （請參閱 圖 1）。 此外，想像一下，電影類別目錄的清單會儲存在資料庫資料表。 在此情況下，它將就從資料庫擷取類別，並呈現檢視主版頁面內的影片分類清單。


[![在 檢視主版頁面中顯示影片類別](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**圖 01**： 顯示在檢視主版頁面的電影類別 ([按一下以檢視完整大小的影像](passing-data-to-view-master-pages-cs/_static/image3.png))


以下是問題。 您要如何擷取主版頁面中的電影類別目錄的清單呢？ 它會嘗試直接呼叫主版頁面中的模型類別的方法。 換句話說，相當吸引人，包括從資料庫權限，您的主版頁面中擷取資料的程式碼。 不過，略過您的 MVC 控制站，來存取資料庫，會違反清楚分離關注點，是建置在 MVC 應用程式的主要優點之一。

在 MVC 應用程式，您會想 MVC 檢視與您的 MVC 模型，由 MVC 控制器之間的所有互動。 此區隔問題會導致更容易維護、 可調適性的且可測試的應用程式。

MVC 應用程式中，傳遞至 – 包括檢視主版頁面 – 檢視的所有資料應都傳遞至檢視的控制器動作。 此外，資料應該傳遞利用檢視資料。 在本教學課程的其餘部分，我會探討兩種方法可以將檢視資料傳遞至檢視主版頁面。

### <a name="the-simple-solution"></a>簡單的解決方案

讓我們開始將檢視資料從控制器傳遞至檢視主版頁面最簡單的解決方案。 最簡單的解決方法是傳入每個控制器動作中的主版頁面的檢視資料。

考量列表 1 中的控制站。 它會公開名為兩個動作`Index()`和`Details()`。 `Index()`動作方法會傳回每個影片的電影資料庫資料表中。 `Details()`動作方法會傳回特定影片類別中的每個影片。

**列表 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

請注意，index （） 和 Details() 動作新增至檢視資料的兩個項目。 Index （） 動作便會新增兩個索引鍵： 類別目錄與電影。 類別索引鍵所代表電影檢視主版頁面所顯示的類別的清單。 影片索引鍵所代表索引檢視頁面所顯示的電影的清單。

Details() 動作也會加入名為分類和電影的兩個索引鍵。 類別索引鍵，同樣地，表示檢視主版頁面所顯示的電影類別目錄的清單。 影片索引鍵所代表的特定類別，顯示 詳細資料檢視 頁面中的電影清單 （請參閱 圖 2）。


[![詳細資料檢視](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**圖 02**: 詳細資料檢視 ([按一下以檢視完整大小的影像](passing-data-to-view-master-pages-cs/_static/image6.png))


索引 檢視會包含在 列表 2。 它只會重複執行的電影中的項目檢視資料所代表的電影清單。

**列表 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

列表 3 中包含的主版頁面的檢視。 檢視主版頁面會逐一查看，並呈現所有電影類別由類別目錄項目從 檢視資料。

**列表 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

若要檢視和檢視主版頁面傳遞所有的資料透過檢視資料。 這是正確的方式將資料傳遞至主版頁面。

因此，什麼是此解決方案有什麼？ 問題在於，此解決方案，違反了 (Don't Repeat Yourself) DRY 準則。 每個控制器動作必須加入相同的檢視資料的電影類別清單。 在您的應用程式中有重複的程式碼，可讓您的應用程式更難維護、 調整及修改。

### <a name="the-good-solution"></a>很好的解決方案

在本節中，我們會檢驗替代，並比較好，解決方案將資料從控制器動作傳遞至檢視主版頁面。 而不是在每個控制器動作中加入主版頁面的電影類別，我們影片將類別新增至檢視資料一次。 為應用程式控制器就會加入所有使用檢視主版頁面的檢視資料。

ApplicationController 類別包含在 列表 4 中。

**列表 4 – `Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

有三個您應該注意到應用程式中的控制站列表 4 相關的項目。 首先，請注意，類別可以繼承自基底 system.web.mvc.controller 衍生類別。 應用程式的控制器是控制器類別。

其次，請注意應用程式控制器類別是抽象類別。 抽象類別是由具象類別必須實作的類別。 應用程式的控制器是抽象類別，因為您不能叫用直接在類別中定義的任何方法。 如果您嘗試直接叫用應用程式類別，您會取得 「 找不到資源 」 錯誤訊息。

第三，要請您注意的是應用程式的控制器包含將加入電影檢視資料的類別清單中的建構函式。 繼承自應用程式的控制站的每個控制器類別會自動呼叫應用程式控制器的建構函式。 每當您呼叫任何繼承自應用程式的控制器的控制站上的任何動作，電影類別是自動包含在檢視資料。

表 5 中的電影控制器繼承自應用程式的控制器。

**列表 5 – `Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

電影控制器，就像在先前的章節中討論的主控制器會公開兩個動作方法，名為`Index()`和`Details()`。 請注意，不是檢視主版頁面所顯示的電影類別目錄的清單加入至檢視中的資料`Index()`或`Details()`方法。 電影控制器是繼承自應用程式的控制器，因為電影類別清單中的加入自動檢視資料。

請注意，此解決方案新增檢視主版頁面的檢視資料不違反 (Don't Repeat Yourself) DRY 準則。 程式碼以新增電影的類別，以檢視資料的清單包含只能在一個位置： 應用程式的控制器的建構函式。

### <a name="summary"></a>總結

在本教學課程中，我們會討論兩種方法可以將檢視資料從控制器傳遞至檢視主版頁面。 首先，我們會檢查簡單，但難以維護的方法。 在第一個區段中，我們會討論如何，您可以在每個每個控制器動作中新增檢視主版頁面的檢視資料在您的應用程式。 我們的結論這是不正確的方法，因為它違反了 (Don't Repeat Yourself) DRY 準則。

接下來，我們會檢查用來加入檢視主版頁面檢視資料所需的資料更好的策略。 而不是在每個控制器動作中新增的檢視資料，我們新增了一次只能檢視資料內的應用程式的控制站。 如此一來，將資料傳遞至檢視主版頁面中的 ASP.NET MVC 應用程式時，可以避免重複的程式碼。

> [!div class="step-by-step"]
> [上一頁](creating-page-layouts-with-view-master-pages-cs.md)
> [下一頁](asp-net-mvc-views-overview-vb.md)
