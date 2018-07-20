---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: 在 GridView 的頁尾 (VB) 顯示摘要資訊 |Microsoft Docs
author: rick-anderson
description: 底部的 摘要的資料列中的報表通常顯示摘要資訊。 GridView 控制項可以包含頁尾資料列至其儲存格中，我們可以提取要求...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: a625211555d0e4351305c92b10559a4019d7e8bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837139"
---
<a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>在 GridView 的頁尾 (VB) 顯示摘要資訊
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe)或[下載 PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> 底部的 摘要的資料列中的報表通常顯示摘要資訊。 GridView 控制項可以包含頁尾資料列至其儲存格中我們以程式設計的方式可將彙總資料。 在本教學課程中，我們會看到此頁尾資料列中顯示彙總資料的方式。


## <a name="introduction"></a>簡介

除了您可以看到每個產品的價格、 庫存單位數 」、 「 單位訂單及重新排列層級上，使用者可能也會想要彙總的資訊，例如平均的價格、 庫存量，總數等等。 這類摘要的資訊通常會顯示在摘要的資料列中的報表底部。 GridView 控制項可以包含頁尾資料列至其儲存格中我們以程式設計的方式可將彙總資料。

這項工作會顯示我們三個挑戰：

1. 設定 GridView，以顯示其頁尾資料列
2. 決定摘要的資料;也就是如何我們計算平均價格或總和的庫存單位數？
3. 插入頁尾資料列的適當的儲存格的摘要資料

在本教學課程中，我們將了解如何克服這些挑戰。 具體來說，我們將建立頁面，其中列出與 GridView 中顯示所選的分類的產品的下拉式清單中的類別。 GridView 會包含在股票和該類別中的產品順序顯示的單位總數與平均價格的頁尾資料列。


[![摘要資訊會顯示在 GridView 的頁尾資料列](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**圖 1**： 摘要資訊會顯示在 GridView 的頁尾資料列 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))


本教學課程中，使用產品的主要/詳細資料介面，其分類為建置基礎的概念稍早所述[主版/詳細篩選使用 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教學課程。 如果您還沒有使用過透過先前的教學課程，請先進行潛在這一個。

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>步驟 1： 加入類別 DropDownList 和產品的 GridView

之前有關自行使用 GridView 的頁尾中加入的摘要資訊，讓我們第一次只要建置主版/詳細報告。 當我們完成第一個步驟之後時，我們將探討如何將加入摘要資料。

首先開啟`SummaryDataInFooter.aspx`頁面中`CustomFormatting`資料夾。 新增 DropDownList 控制項並設定其`ID`至`Categories`。 接下來，按一下 從 DropDownList 的智慧標籤的 選擇資料來源 連結，並選擇新增名為新 ObjectDataSource`CategoriesDataSource`叫用`CategoriesBLL`類別的`GetCategories()`方法。


[![新增名為 CategoriesDataSource 新 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**圖 2**： 新增新的 ObjectDataSource 名為`CategoriesDataSource`([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))


[![已叫用 CategoriesBLL 類別的 GetCategories() 方法的 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**圖 3**： 有 ObjectDataSource 叫用`CategoriesBLL`類別的`GetCategories()`方法 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))


之後設定 ObjectDataSource，精靈會傳回我們 DropDownList 的資料來源組態精靈 」，我們要指定哪些資料欄位值應該會顯示，而且其中一個應該對應至 DropDownList 值的`ListItem` s。 已`CategoryName`顯示的欄位，並使用`CategoryID`做為值。


[![分別為文字和值 ListItems，使用類別名稱和 CategoryID 欄位](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**圖 4**： 使用`CategoryName`並`CategoryID`視欄位`Text`並`Value`的`ListItem`s，分別 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))


現在我們有 DropDownList (`Categories`)，在系統中列出的類別。 我們現在需要新增的 GridView 會列出這些屬於所選分類的產品。 這樣做，不過，先花點時間檢查 DropDownList 的智慧標籤啟用 AutoPostBack 核取方塊。 中所述*主版/詳細篩選使用 DropDownList*教學課程中，藉由設定 DropDownList`AutoPostBack`屬性設`True`頁面將會公佈回的每當 DropDownList 值變更時。 這會導致重新整理，GridView 顯示這些產品的新選取的分類。 如果`AutoPostBack`屬性設定為`False`（預設值），變更類別目錄不會造成回傳並不會更新列出的產品。


[![核取方塊啟用 AutoPostBack 在 DropDownList 的智慧標籤](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**圖 5**： 核取方塊啟用 AutoPostBack DropDownList 的智慧標籤中 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))


將 GridView 控制項加入頁面，以顯示所選分類的產品。 設定 GridView`ID`要`ProductsInCategory`並將它繫結至名為新 ObjectDataSource `ProductsInCategoryDataSource`。


[![新增名為 ProductsInCategoryDataSource 新 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**圖 6**： 新增新的 ObjectDataSource 名為`ProductsInCategoryDataSource`([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))


因此，它會叫用設定 ObjectDataSource`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。


[![已叫用 GetProductsByCategoryID(categoryID) 方法的 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**圖 7**： 有 ObjectDataSource 叫用`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))


因為`GetProductsByCategoryID(categoryID)`方法接受一個輸入參數，在精靈的最後一個步驟中，我們可以指定參數值的來源。 若要顯示這些產品，從選取的類別，具有 取自參數`Categories`DropDownList。


[![從選取類別 DropDownList 取得 categoryID 參數值](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**圖 8**： 取得*`categoryID`* 參數值，從選取類別 DropDownList ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))


完成精靈之後 GridView 將會有 BoundField 每個產品的屬性。 讓我們清除這些 BoundFields 使得只有`ProductName`， `UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`BoundFields 會顯示。 歡迎您將任何欄位層級設定新增至其餘 BoundFields (格式化等`UnitPrice`為貨幣)。 進行這些變更之後，GridView 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

此時，我們會有完整的主要/詳細資料報表，顯示名稱、 單位價格、 庫存量和單位，屬於所選分類這些產品的訂購。


[![從選取類別 DropDownList 取得 categoryID 參數值](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**圖 9**： 取得*`categoryID`* 參數值，從選取類別 DropDownList ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>步驟 2： 顯示在 GridView 的頁尾

GridView 控制項可以顯示頁首和頁尾資料列。 這些資料列會顯示依據的值`ShowHeader`並`ShowFooter`屬性，分別使用`ShowHeader`預設為`True`並`ShowFooter`來`False`。 若要直接包含在 GridView 的頁尾設定其`ShowFooter`屬性設`True`。


[![GridView 的 ShowFooter 屬性設定為 True](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**圖 10**： 設定 GridView`ShowFooter`屬性設`True`([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))


頁尾資料列具有每個子 GridView; 內所定義之欄位的資料格不過，這些資料格預設為空白。 請花一點時間瀏覽器中檢視進度。 具有`ShowFooter`內容現在設定為`True`，GridView 包含空的頁尾資料列。


[![GridView 現在包含頁尾資料列](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**圖 11**： 現在包含 GridView 的頁尾資料列 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))


[圖 11] 中的頁尾資料列不脫穎而出，因為它具有白色背景。 讓我們來建立`FooterStyle`CSS 類別`Styles.css`指定為深紅色背景，然後設定`GridView.skin`面板中的檔案`DataWebControls`GridView 的單位為這個 CSS 類別的佈景主題`FooterStyle`的`CssClass`屬性。 如果您要溫習面板和佈景主題，請參閱上一步[顯示的資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)教學課程。

新增下列 CSS 類別開始`Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

`FooterStyle` CSS 類別很類似，在樣式`HeaderStyle`類別，雖然`HeaderStyle`的背景色彩較暗的微妙影響，且它的文字會顯示以粗體字。 此外，在頁尾中的文字會靠右對齊而標頭的文字置中。

接下來，若要建立這個 CSS 類別的關聯與每個 GridView 的頁尾，開啟`GridView.skin`檔案中`DataWebControls`佈景主題和 set`FooterStyle`的`CssClass`屬性。 在此新增後檔案的標記應該看起來像：


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

這項變更如螢幕擷取畫面如下所示，讓頁尾清楚地凸顯更多。


[![GridView 的頁尾資料列現在有紅的背景色彩](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**圖 12**: GridView 的頁尾資料列現在有紅的背景色彩 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>步驟 3︰ 計算摘要資料

使用 GridView 的頁尾中顯示，接下來我們面臨的挑戰是如何計算摘要資料。 有兩種方式來計算此彙總的資訊：

1. 透過 SQL 查詢中，我們可以發出額外的查詢資料庫以計算特定類別的摘要資料。 SQL 包含數個彙總函式，連同`GROUP BY`子句來指定哪些資料應該彙總的資料。 下列 SQL 查詢會回復所需的資訊：  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    當然您不想要發出這項查詢，直接從`SummaryDataInFooter.aspx`頁面上，而是藉由建立中的方法`ProductsTableAdapter`而`ProductsBLL`。
2. 計算這項資訊，因為它正在加入至 GridView 中所述[自訂格式設定基礎時的資料](custom-formatting-based-upon-data-cs.md)教學課程中，GridView 的`RowDataBound`事件處理常式，就會引發一次每個資料列新增至後 GridView 其已資料繫結。 藉由建立此事件的事件處理常式中，我們可以把執行我們想要的值的總計彙總。 最後一個之後的資料列已經繫結至 GridView，我們有總計和計算平均值時，所需的資訊。

我通常採用第二種方法，因為它會將某趟車程支付儲存至資料庫，以及資料存取層和商務邏輯層中實作 「 摘要 」 功能所需的心力，但兩種方法即已足夠。 本教學課程中讓我們使用第二個選項，並累計總數的使用追蹤的`RowDataBound`事件處理常式。

建立`RowDataBound`事件處理常式，方法是將在設計工具中選取 GridView 中，按一下閃電圖示從 [屬性] 視窗中，按兩下 gridview`RowDataBound`事件。 或者，您可以從 ASP.NET 程式碼後置類別檔案頂端的下拉式清單選取 GridView 以及其 RowDataBound 事件。 這會建立名為新的事件處理常式`ProductsInCategory_RowDataBound`在`SummaryDataInFooter.aspx`頁面的程式碼後置類別。


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

為了維護執行總數中，我們必須定義變數超出範圍的事件處理常式。 建立下列四個頁面層級變數：

- `_totalUnitPrice`型別 `Decimal`
- `_totalNonNullUnitPriceCount`型別 `Integer`
- `_totalUnitsInStock`型別 `Integer`
- `_totalUnitsOnOrder`型別 `Integer`

接下來，撰寫程式碼中遇到的每個資料列的遞增這三個變數`RowDataBound`事件處理常式。


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

`RowDataBound`確保我們要處理的 DataRow 的事件處理常式會啟動。 一旦已建立的`Northwind.ProductsRow`只是繫結至執行個體`GridViewRow`物件中`e.Row`儲存在變數`product`。 接下來，執行的總變數目前的產品對應的值以遞增 (假設它們不包含資料庫`NULL`值)。 我們持續追蹤的這兩個執行`UnitPrice`總計和非數字`NULL``UnitPrice`記錄因為平均的價格是這兩個數字的商數。

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>步驟 4： 在頁尾顯示摘要資料

總計的摘要資料，最後一個步驟是在 GridView 的頁尾資料列中顯示它。 這項工作，也可完成以程式設計方式透過`RowDataBound`事件處理常式。 請記得，`RowDataBound`針對觸發的事件處理常式*每個*繫結至 GridView，包括頁尾資料列的資料列。 因此，我們可以加強我們的事件處理常式，來顯示頁尾資料列，使用下列程式碼中的資料：


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

因為頁尾資料列加入至 GridView 之後新增所有的資料列之後，我們可以確信時我們已準備好可以顯示在頁尾中的累計總數的計算會完成的摘要資料。 最後一個步驟中，則，是在頁尾中的資料格中設定這些值。

若要在特定的頁尾資料格中顯示文字，請使用`e.Row.Cells(index).Text = value`，其中`Cells`編製索引從 0 開始。 下列程式碼計算平均價格 （總計除以的數字的產品的價格），並顯示它的單位總數以及股票圖和 GridView 的頁尾適當儲存格中的訂購量。


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

已加入此程式碼之後，圖 13 顯示的報表。 請注意如何`ToString("c")`平均價格的摘要資訊格式化貨幣等。


[![GridView 的頁尾資料列現在有紅的背景色彩](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**圖 13**: GridView 的頁尾資料列現在有紅的背景色彩 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))


## <a name="summary"></a>總結

顯示摘要資料是報表的一般需求，和 GridView 控制項可讓您輕鬆地在其頁尾資料列中包含這類資訊。 顯示頁尾資料列時 GridView`ShowFooter`屬性設定為`True`可以在設定以程式設計方式透過其儲存格的文字和`RowDataBound`事件處理常式。 計算摘要資料是可重新查詢資料庫，或使用 ASP.NET 網頁的程式碼後置類別中的程式碼，以程式設計方式計算摘要資料。

本教學課程結束時，我們檢查與 GridView、 DetailsView 和 FormView 控制項的自訂格式。 我們的下一個教學課程一開始我們探勘的插入、 更新和刪除使用這些相同的控制項的資料。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一步](using-the-formview-s-templates-vb.md)
