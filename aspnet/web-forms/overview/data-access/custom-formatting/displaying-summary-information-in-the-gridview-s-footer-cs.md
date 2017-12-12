---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: "在 GridView 的頁尾 (C#) 中顯示摘要資訊 |Microsoft 文件"
author: rick-anderson
description: "底部的摘要資料列中的報表通常顯示摘要資訊。 GridView 控制項可以包含頁尾資料列至其儲存格，我們可以 pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d3df976181a4641dbfffe77875989c77ece059d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>在 GridView 的頁尾 (C#) 中顯示摘要資訊
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe)或[下載 PDF](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> 底部的摘要資料列中的報表通常顯示摘要資訊。 GridView 控制項可以包含頁尾資料列至其儲存格以程式設計的方式可以插入彙總資料。 在本教學課程，我們會看到如何顯示此頁尾資料列中的彙總資料。


## <a name="introduction"></a>簡介

除了查看順序，以及重新訂購層級上的每個產品的價格、 庫存、 單位，使用者可能也會想要彙總的資訊，例如平均價格，庫存的總數等等。 這類的摘要資訊通常會顯示摘要資料列中的報表底部。 GridView 控制項可以包含頁尾資料列至其儲存格以程式設計的方式可以插入彙總資料。

這項工作會顯示我們與這三個挑戰：

1. 設定要顯示其頁尾資料列 GridView
2. 決定摘要的資料;也就是如何我們計算平均價格或庫存單位總數？
3. 摘要資料插入到適當的儲存格的頁尾資料列

在本教學課程中，我們會看到如何克服這些挑戰。 具體來說，我們將建立頁面，其中列出與顯示在 GridView 中選取類別目錄的產品下拉式清單中的類別。 在 GridView 會包含頁尾資料列，其中顯示平均價格和之單元的總數，庫存和產品類別目錄中的順序。


[![摘要資訊會顯示在 GridView 的頁尾資料列](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**圖 1**： 摘要資訊會顯示在 GridView 的頁尾資料列 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))


此教學課程中，其產品的主要/詳細資料介面的類別目錄與根據先前所涵蓋的概念[主要/詳細資料篩選與 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教學課程。 如果您已經還會執行較早的教學課程，請先進行繼續在這一個。

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>步驟 1： 加入 DropDownList 分類和產品的 GridView

之前有關自己新增至 GridView 的頁尾的摘要資訊，讓我們第一次只建立主要/詳細資料報表。 當我們完成第一個步驟之後時，我們會探討如何將加入摘要資料。

先開啟`SummaryDataInFooter.aspx`頁面`CustomFormatting`資料夾。 將 DropDownList 控制項，並設定其`ID`至`Categories`。 接下來，按一下 選擇資料來源連結，從 DropDownList 的智慧標籤上，選擇加入名為新 ObjectDataSource`CategoriesDataSource`會叫用`CategoriesBLL`類別的`GetCategories()`方法。


[![加入名為 CategoriesDataSource 新 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**圖 2**： 加入新的 ObjectDataSource 名為`CategoriesDataSource`([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))


[![已叫用 CategoriesBLL 類別 GetCategories() 方法 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**圖 3**： 有 ObjectDataSource 叫用`CategoriesBLL`類別的`GetCategories()`方法 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))


設定後 ObjectDataSource，精靈就會返回我們 DropDownList 的資料來源組態精靈，我們要指定哪些資料欄位值應該會顯示與哪一個應該對應至的 DropDownList 的值`ListItem` s。 具有`CategoryName`顯示欄位和使用`CategoryID`做為值。


[![分別為文字和值 ListItems，使用類別名稱和 CategoryID 欄位](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**圖 4**： 使用`CategoryName`和`CategoryID`視欄位`Text`和`Value`如`ListItem`s，分別 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))


現在我們有 DropDownList (`Categories`) 的系統中列出的類別。 我們現在要加入的 GridView 會列出這些屬於所選類別目錄的產品。 我們執行動作，不過前, 先花點時間選取 DropDownList 的智慧標籤中啟用 AutoPostBack 核取方塊。 中所述*主要/詳細資料篩選與 DropDownList*教學課程，藉由設定 DropDownList`AutoPostBack`屬性`true`頁面將會公佈回每次 DropDownList 值有所變更。 這會導致重新整理 GridView 顯示新選取的類別目錄的產品。 如果`AutoPostBack`屬性設定為`false`（預設值），變更類別目錄不會造成回傳，並因此將不會更新所列的產品。


[![選取 DropDownList 的智慧標籤中啟用 AutoPostBack 核取方塊](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**圖 5**： 核取啟用 AutoPostBack DropDownList 的智慧標籤中 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))


加入至頁面的 GridView 控制項，才能顯示所選類別目錄的產品。 設定 GridView`ID`至`ProductsInCategory`和繫結到名為新 ObjectDataSource `ProductsInCategoryDataSource`。


[![加入名為 ProductsInCategoryDataSource 新 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**圖 6**： 加入新的 ObjectDataSource 名為`ProductsInCategoryDataSource`([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))


因此，它會叫用設定 ObjectDataSource`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。


[![已叫用 GetProductsByCategoryID(categoryID) 方法 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**圖 7**： 有 ObjectDataSource 叫用`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))


因為`GetProductsByCategoryID(categoryID)`方法接受輸入參數，在精靈的最後一個步驟中，我們可以指定參數值的來源。 若要顯示這些產品，從選取的類別，具有 取自參數`Categories`DropDownList。


[![從選取類別 DropDownList 取得 categoryID 參數值](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**圖 8**： 取得 *`categoryID`* 參數值，從選取類別 DropDownList ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))


在精靈完成後 GridView 將會有 BoundField 每個產品的屬性。 讓我們來清除這些 BoundFields 使得只有`ProductName`， `UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`BoundFields 會顯示。 歡迎剩餘 BoundFields 中加入任何欄位層級設定 (例如格式化`UnitPrice`為貨幣)。 進行這些變更之後，GridView 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

現在我們有正常運作的主要/詳細資料報表，屬於所選類別目錄的產品的順序顯示名稱、 單位價格、 庫存量和單位。


[![從選取類別 DropDownList 取得 categoryID 參數值](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**圖 9**： 取得 *`categoryID`* 參數值，從選取類別 DropDownList ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>步驟 2： 在 GridView 中顯示頁尾

GridView 控制項可以顯示頁首和頁尾資料列。 這些資料列會顯示依據的值`ShowHeader`和`ShowFooter`屬性，分別與`ShowHeader`預設為`true`和`ShowFooter`至`false`。 若要在 GridView 中只包含頁尾設定其`ShowFooter`屬性`true`。


[![設定為 true 的 GridView ShowFooter 屬性](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**圖 10**： 設定 GridView`ShowFooter`屬性`true`([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))


頁尾資料列會有一個資料格的每個 GridView; 中定義的欄位不過，這些資料格預設為空白。 請花一點時間瀏覽器中檢視進度。 與`ShowFooter`內容現在設定為`true`，GridView 包含空白的頁尾資料列。


[![GridView 現在包含頁尾資料列](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**圖 11**: GridView 現在包含頁尾資料列 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))


圖 11 頁尾資料列不會突顯出來，因為它具有白色背景。 讓我們來建立`FooterStyle`CSS 類別`Styles.css`，指定深紅色背景，然後設定`GridView.skin`面板檔案中的`DataWebControls`GridView 的單位為這個 CSS 類別的佈景主題`FooterStyle`的`CssClass`屬性。 如果您需要複習面板和佈景主題，請參閱上一步[顯示資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)教學課程。

藉由新增下列 CSS 類別以啟動`Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

`FooterStyle` CSS 類別是類似的樣式`HeaderStyle`類別，雖然`HeaderStyle`的背景色彩越深微妙的地方，並以粗體字顯示其文字。 此外，頁尾中的文字會靠右對齊而標頭的文字置中。

接下來，若要將這個 CSS 類別與每個 GridView 頁尾產生關聯，開啟`GridView.skin`檔案`DataWebControls`佈景主題和集`FooterStyle`的`CssClass`屬性。 在此新增之後的檔案標記看起來應該像：


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

如螢幕擷取畫面所示，這項變更可讓您在頁尾突顯更清楚。


[![在 GridView 的頁尾資料列現在有紅的背景色彩](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**圖 12**: GridView 頁尾資料列現在有紅的背景色彩 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>步驟 3： 計算摘要資料

與顯示的 GridView 的頁尾，我們對向的下一步挑戰是如何計算摘要資料。 有兩種方式來計算此彙總的資訊：

1. 透過 SQL 查詢中，我們無法發出的其他查詢資料庫以計算某個特定類別的摘要資料。 SQL 包含數個彙總函式，連同`GROUP BY`子句來指定哪些資料應該彙總的資料。 下列 SQL 查詢會回復所需的資訊：  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    當然您不想要發行此查詢，直接從`SummaryDataInFooter.aspx`頁面上，而是藉由建立中的方法`ProductsTableAdapter`和`ProductsBLL`。
2. 計算這項資訊，因為它正在加入至 GridView 中所述[自訂格式化時資料](custom-formatting-based-upon-data-cs.md)教學課程中，在 GridView 的`RowDataBound`事件處理常式會在每個資料列之後 GridView 加入一次引發其已資料繫結。 藉由建立此事件的事件處理常式中，我們會保留所執行的總我們想要的值的彙總。 最後一個之後的資料列已經繫結至 GridView 總計和計算平均時所需的資訊。

我通常採用第二種方法，它會將路線儲存至資料庫，以及商務邏輯層和資料存取層中實作的摘要功能所需的投入時間，但兩種方法即已足夠。 此教學課程中我們使用第二個選項，並且追蹤累計總數使用`RowDataBound`事件處理常式。

建立`RowDataBound`GridView 選取設計工具中，按一下閃電圖示從 [屬性] 視窗中，按兩下 GridView 的事件處理常式`RowDataBound`事件。 這會建立新的事件處理常式，名為`ProductsInCategory_RowDataBound`中`SummaryDataInFooter.aspx`網頁的程式碼後置類別。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

為了維護執行總數中，我們需要定義變數超出範圍的事件處理常式。 建立下列四個的頁面層級變數：

- `_totalUnitPrice`型別`decimal`
- `_totalNonNullUnitPriceCount`型別`int`
- `_totalUnitsInStock`型別`int`
- `_totalUnitsOnOrder`型別`int`

接下來，撰寫程式碼中的每個資料列時發生遞增這些三個變數`RowDataBound`事件處理常式。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

`RowDataBound`事件處理常式一開始會確保我們正在處理的資料列。 一旦已建立的`Northwind.ProductsRow`只繫結至的執行個體`GridViewRow`物件存放至`e.Row`儲存在變數`product`。 接下來，執行的總變數目前產品之對應值以遞增 (假設它們不包含資料庫`NULL`值)。 我們會持續追蹤的兩個執行`UnitPrice`總計和非數字`NULL``UnitPrice`記錄因為平均的價格是兩個數值的商數。

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>步驟 4： 在頁尾中顯示摘要資料

總計的摘要資料，最後一個步驟是在 GridView 的頁尾資料列中顯示。 這項工作，也可以完成以程式設計方式透過`RowDataBound`事件處理常式。 請記得，`RowDataBound`事件處理常式引發的*每*繫結至 GridView，包括頁尾資料列的資料列。 因此，我們可以加強我們要使用下列程式碼的頁尾資料列中顯示資料的事件處理常式：


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

因為頁尾資料列加入至 GridView 加入所有資料列後，我們可以確信時由我們已準備好可以在將已完成的累計總數的計算頁尾顯示摘要資料。 最後一個步驟中，接著是頁尾的資料格中設定這些值。

若要在特定的頁尾資料格中顯示文字，請使用`e.Row.Cells[index].Text = value`，其中`Cells`索引 0 開始。 下列程式碼會計算平均價格 （總價除以產品數目），並顯示之單元的總數以及在股票圖和單位上的 GridView 的適當頁尾資料格的順序。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

圖 13 在已加入此程式碼之後顯示的報表。 請注意如何`ToString("c")`像貨幣格式化，平均價格摘要資訊。


[![在 GridView 的頁尾資料列現在有紅的背景色彩](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**圖 13**: GridView 頁尾資料列現在有紅的背景色彩 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))


## <a name="summary"></a>總結

顯示摘要資料是報表的一般需求，和 GridView 控制項可讓您輕鬆地在其頁尾資料列中包含這類資訊。 顯示頁尾資料列時的 GridView`ShowFooter`屬性設定為`true`，而且可以透過程式設計方式設定其儲存格有文字`RowDataBound`事件處理常式。 計算摘要資料可能可以藉由重新查詢資料庫，或使用 ASP.NET 網頁的程式碼後置類別中的程式碼，來以程式設計方式計算摘要資料。

本教學課程結束時，我們檢驗使用 GridView、 DetailsView 和在 FormView 的控制項的自訂格式。 我們的下一個教學課程一開始我們瀏覽的插入、 更新和刪除資料，使用這些相同的控制項。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一頁](using-the-formview-s-templates-cs.md)
[下一頁](custom-formatting-based-upon-data-vb.md)
