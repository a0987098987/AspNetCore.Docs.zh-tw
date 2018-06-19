---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: 將資料傳遞至檢視主版頁面 (VB) |Microsoft 文件
author: microsoft
description: 本教學課程的目標是要說明如何將資料從控制器檢視主版頁面。 我們會檢查兩種策略將資料傳遞至檢視 m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: fcd7c5baacc00490720d1f82252d81e40c097c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870748"
---
<a name="passing-data-to-view-master-pages-vb"></a>將資料傳遞至檢視主版頁面 (VB)
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> 本教學課程的目標是要說明如何將資料從控制器檢視主版頁面。 我們會檢查兩種策略將資料傳遞至檢視主版頁面。 首先，我們會討論簡單的解決方案所產生的應用程式，很難以維護。 接下來，我們會檢查比較好的解決方案有點需要更多的初始工作，但更容易維護的應用程式中的結果。


## <a name="passing-data-to-view-master-pages"></a>將資料傳遞至檢視主版頁面

本教學課程的目標是要說明如何將資料從控制器檢視主版頁面。 我們會檢查兩種策略將資料傳遞至檢視主版頁面。 首先，我們會討論簡單的解決方案所產生的應用程式，很難以維護。 接下來，我們會檢查比較好的解決方案有點需要更多的初始工作，但更容易維護的應用程式中的結果。

### <a name="the-problem"></a>問題

假設您要建置影片資料庫應用程式，而您想要在應用程式中的每一頁上顯示電影類別目錄的清單 （請參閱圖 1）。 此外，假設影片類別目錄的清單，會儲存在資料庫資料表。 在此情況下，它會使意義上，從資料庫擷取類別，並呈現檢視主版頁面中的影片分類清單。


[![顯示在檢視主版頁面的電影類別](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**圖 01**： 電影類別顯示在檢視主版頁面 ([按一下以檢視完整大小的影像](passing-data-to-view-master-pages-vb/_static/image3.png))


以下是此問題。 如何擷取主版頁面中的影片類別目錄的清單？ 它會嘗試直接呼叫主版頁面中的模型類別的方法。 換句話說，很容易包含從您的主版頁面的資料庫右邊擷取資料的程式碼。 不過，略過您存取資料庫的 MVC 控制器會違反全新的重要性分離所建立的 MVC 應用程式的主要優點之一。

在 MVC 應用程式，您會想 MVC 檢視與 MVC 模型，由 MVC 控制器之間的所有互動。 此分離問題會導致更容易維護、 可調適性，且可測試的應用程式。

MVC 應用程式中，所有的資料傳遞至 – 包括檢視主版頁面 – 檢視應該傳遞至檢視的控制器動作。 此外，資料應該傳遞利用檢視資料。 在本教學課程的其餘部分，我會檢查傳遞給檢視主版頁面的 檢視資料的兩種方法。

### <a name="the-simple-solution"></a>簡單的解決方案

讓我們從開始檢視主版頁面，從控制器傳遞檢視資料最簡單的解決方案。 最簡單的解決方案是在每個控制器動作中傳遞的主版頁面的檢視資料。

請考慮列出 1 中的控制站。 它會公開兩個動作，名為`Index()`和`Details()`。 `Index()`動作方法會傳回每個影片電影資料庫資料表中。 `Details()`動作方法會傳回每個影片中特定的影片類別目錄。

**列出 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

請注意，同時`Index()`和`Details()`動作加入至檢視資料的兩個項目。 `Index()`動作會將兩個索引鍵： 類別目錄與電影。 類別索引鍵代表檢視主版頁面所顯示的電影類別目錄的清單。 電影索引鍵代表索引檢視頁面所顯示的電影的清單。

`Details()`動作也會加入名為類別目錄與電影的兩個索引鍵。 索引鍵的類別，同樣地，表示檢視主版頁面所顯示的電影類別目錄的清單。 電影索引鍵所代表的特定分類的詳細資料檢視頁面所顯示的電影清單 （請參閱圖 2）。


[![詳細資料檢視](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**圖 02**: 詳細資料檢視 ([按一下以檢視完整大小的影像](passing-data-to-view-master-pages-vb/_static/image6.png))


索引檢視包含在清單 2。 它只是逐一查看的電影中的項目檢視資料所代表的電影清單。

**列出 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

檢視主版頁面都包含在列出的 3。 檢視主版頁面反覆運算，並呈現所有以類別目錄項目表示從檢視資料的電影類別。

**列出 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

所有的資料會透過檢視資料傳遞至檢視和檢視主版頁面。 這是正確的方式將資料傳遞至主版頁面。

因此，什麼是此解決方案有什麼？ 問題是這個解決方案違反乾 （不重複自行） 原則。 每個控制器動作必須完全相同之新增電影的分類，以檢視資料。 在您的應用程式中具有重複的程式碼，可讓您的應用程式更難維護、 調整，以及修改。

### <a name="the-good-solution"></a>很好的解決方案

在本節中，我們會檢查替代，以及更好，解決方案將資料從控制器動作傳遞至檢視主版頁面。 而非新增主版頁面的電影分類中每個控制器動作，我們加入電影類別檢視資料一次。 使用檢視主版頁面的所有檢視資料會都加入應用程式控制器。

ApplicationController 類別被包含在列出的 4。

ApplicationController 類別被包含在列出的 4。

**列出 4 – `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

有三個的項目，您應該注意到相關的應用程式中的控制站清單 4。 首先，請注意，類別可以繼承自基底 System.Web.Mvc.Controller 類別。 應用程式的控制站是控制器的類別。

第二，請注意，應用程式控制器類別 MustInherit 類別。 MustInherit 類別是具象類別必須實作的類別。 因為應用程式的控制站是 MustInherit 類別，您無法叫用直接定義在類別中的任何方法。 如果您嘗試直接叫用應用程式類別，您會取得 「 找不到資源 」 錯誤訊息。

第三個，請注意，應用程式控制器包含將加入電影來檢視資料的類別清單中的建構函式。 繼承自應用程式的控制站的每個控制器類別會自動呼叫應用程式控制器的建構函式。 每當您呼叫任何繼承自應用程式控制器的控制站上的任何動作時，電影類別會自動包含在檢視資料。

電影中的控制站列出 5 繼承自應用程式的控制站。

**列出 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

電影控制器，就像在先前的章節中討論的主控制器會公開兩個動作方法，名為`Index()`和`Details()`。 請注意，不是檢視主版頁面所顯示的電影類別目錄的清單加入至檢視中的資料`Index()`或`Details()`方法。 電影控制器繼承自應用程式的控制站，所以影片類別清單中的加入自動檢視資料。

請注意此解決方案，以新增檢視主版頁面的檢視資料沒有違反乾 （不重複自行） 原則。 程式碼以新增電影的分類，以檢視資料的清單包含只能在一個位置： 應用程式的控制站的建構函式。

### <a name="summary"></a>總結

在本教學課程中，我們將討論兩種方法來將從控制器的檢視資料傳遞至檢視主版頁面。 首先，我們會檢查簡單，但難以維護的方法。 在第一個區段中，我們將討論如何新增檢視主版頁面的檢視資料中每個每個控制器動作在應用程式中。 我們可以得到這是不正確的方法，因為它違反乾 （不重複自行） 原則。

接下來，我們會檢查較佳策略來加入資料檢視主版頁面才能檢視資料。 而不是新增的檢視資料中每個控制器動作，我們會加入一次檢視資料中的應用程式的控制站。 這樣一來，將資料傳遞到 ASP.NET MVC 應用程式中檢視主版頁面時，可以避免重複的程式碼。

> [!div class="step-by-step"]
> [上一步](creating-page-layouts-with-view-master-pages-vb.md)
