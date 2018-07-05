---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: 主版/詳細篩選使用 dropdownlist 進行 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們會看到如何顯示在 DropDownList 控制項和 GridView 中選取的清單項目的詳細資料的主要記錄。
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: a1bd1a20950376244c1d461d139f3eee6bc9a9cc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816252"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>主版/詳細篩選使用 dropdownlist 進行 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe)或[下載 PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> 在本教學課程中，我們會看到如何顯示在 DropDownList 控制項和 GridView 中選取的清單項目的詳細資料的主要記錄。


## <a name="introduction"></a>簡介

是一種常見的報表*主要/詳細資料報表*，在其報表一開始會顯示某些的 「 主要 」 的記錄集。 使用者可以再向下切入到其中一個主要記錄中，藉此檢視的主要記錄的 [詳細資料。] 主版/詳細報告是理想的選擇，以視覺化方式呈現一對多關聯性，例如報表顯示所有類別，然後由使用者選取特定的類別，並顯示其相關聯的產品。 此外，主版/詳細報告也適用於顯示特別 「 寬的 」 的資料表 （亦即有許多資料行） 的詳細的資訊。 比方說，主要/詳細資料報表的 「 主要 」 層級可能會在資料庫中，顯示只是產品名稱和單位價格的產品，並向下切入至特定的產品會顯示額外的產品欄位 (類別、 供應商、 每個單位，數量和等等）。

有許多方式可以實作主從式報表。 這和接下來三個教學課程將探討各種不同的主版/詳細報告。 在本教學課程中，我們會看到如何顯示中的主要記錄[DropDownList 控制項](https://msdn.microsoft.com/library/dtx91y0z.aspx)和 GridView 中選取的清單項目的詳細資料。 特別是，本教學課程的主要/詳細資料報表會列出類別目錄和產品資訊。

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>步驟 1： 顯示在 DropDownList 中的類別

我們的主要/詳細資料報表會列出的類別中的下拉式清單中，與顯示的選取的清單項目的產品 [GridView] 頁面中進一步向下。 超越我們，第一項工作，則將顯示在 DropDownList 中的類別。 開啟`FilterByDropDownList.aspx`頁面中`Filtering`資料夾中，從 [工具箱] 拖曳至頁面的設計工具，將在 dropdownlist 進行，然後設定其`ID`屬性設`Categories`。 接下來，按一下 從 DropDownList 的智慧標籤的 選擇資料來源 連結。 這會顯示 資料來源組態精靈。


[![指定 DropDownList 的資料來源](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**圖 1**： 指定 DropDownList 的資料來源 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


選擇新增名為新 ObjectDataSource`CategoriesDataSource`叫用`CategoriesBLL`類別的`GetCategories()`方法。


[![新增名為 CategoriesDataSource 新 ObjectDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**圖 2**： 新增新的 ObjectDataSource 名為`CategoriesDataSource`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![選擇使用 CategoriesBLL 類別](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**圖 3**： 選擇使用`CategoriesBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![設定為使用 GetCategories() 方法的 ObjectDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**圖 4**： 設定要使用 ObjectDataSource`GetCategories()`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


在設定我們仍然需要指定哪些資料來源欄位應該會顯示在 DropDownList 和 ObjectDataSource 後其中一個應該做為清單項目的值相關聯。 已`CategoryName`欄位做為顯示和`CategoryID`做為每個清單項目的值。


[![做為值的類別名稱 欄位和使用 CategoryID 有 DropDownList 顯示](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**圖 5**: DropDownList 顯示`CategoryName`欄位並使用`CategoryID`做為值 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


現在我們有 DropDownList 控制項，其中會填入來自記錄`Categories`資料表 （全部在大約六秒內完成）。 圖 6 顯示我們進行到目前為止透過瀏覽器檢視時。


[![下拉式清單會列出目前的類別](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**圖 6**: 下拉式清單會列出目前的類別 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>步驟 2： 加入產品 GridView

在我們的主要/詳細資料報表中的最後一個步驟是列出與選取的類別相關聯的產品。 若要這麼做，加入至頁面的 GridView，並建立名為新 ObjectDataSource `productsDataSource`。 已`productsDataSource`控制挑選其資料，從`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。


[![選取 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**圖 7**： 選取 `GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


選擇這個方法之後, ObjectDataSource 精靈會提示我們輸入方法的值*`categoryID`* 參數。 若要使用選取的值`categories`DropDownList 項目設定參數來源控制與以 ControlID `Categories`。


[![設定為值的分類 DropDownList categoryID 參數](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**圖 8**： 設定*`categoryID`* 參數的值`Categories`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


花點時間查看我們的瀏覽器中的進度。 當第一次瀏覽的頁面，這些產品屬於所選取的類別 （如 圖 9 所示），顯示 （飲料），但變更 DropDownList 不會更新資料。 這是因為更新 GridView 會因發生回傳。 若要這麼做，我們會有兩個選項 （兩者都不需要撰寫任何程式碼）：

- **設定類別 DropDownList**[AutoPostBack 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**設為 True。** （您可以完成這藉由檢查 DropDownList 的智慧標籤的 [啟用 AutoPostBack] 選項。）這會觸發回傳，每當 DropDownList 的選取項目由使用者變更。 因此，當使用者選取新的類別從 DropDownList 接踵而至回傳而 GridView 會更新為新選取的分類的產品。 （這是我在本教學課程使用的方法）。
- **加入 Button Web 控制項旁 DropDownList。** 設定其`Text`屬性，以重新整理或類似。 使用此方法時，使用者必須選取新的類別，然後按一下  按鈕。 按一下按鈕，將會導致回傳，並更新 GridView，以列出所選類別的這些產品。

圖 9 和 10 說明主要/詳細資料報表動作中。


[![當第一次瀏覽的頁面，會顯示飲料產品](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**圖 9**： 當第一次瀏覽的頁面，會顯示飲料產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![選取新的產品 （產生） 會自動造成回傳，更新 GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**圖 10**： 選取新的產品 （產生） 會自動造成回傳，更新 GridView ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>新增"--選擇類別目錄-」 清單項目

第一次瀏覽時`FilterByDropDownList.aspx`頁面上選取 DropDownList 的第一個清單項目 （飲料） 根據預設，在 GridView 中顯示的飲料產品類別目錄。 而不是顯示第一個類別目錄的產品，我們可能想要改為具有 DropDownList 項目選取，顯示類似，「-選擇類別目錄-」。

若要將新的清單項目新增至 DropDownList 中，移至 [屬性] 視窗，然後按一下省略符號，`Items`屬性。 加入新的清單項目具有`Text`"--選擇類別目錄-"和`Value` `-1`。


[![新增將--選擇類別目錄--清單項目](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**圖 11**： 新增將--選擇類別目錄--清單項目 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


或者，您也可以藉由將下列標記新增至 DropDownList 新增清單項目：


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

此外，我們需要設定 DropDownList 控制項`AppendDataBoundItems`設為 True，因為當類別繫結至 DropDownList 從 ObjectDataSource 它們將會覆寫任何手動加入的清單項目如果`AppendDataBoundItems`不是 True。


![AppendDataBoundItems 屬性設定為 True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**圖 12**： 設定`AppendDataBoundItems`屬性設為 True


這些變更之後，請先瀏覽的頁面已選取 [-選擇類別目錄-] 選項，並不顯示任何產品時。


[![沒有產品都會顯示在初始頁面載入](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**圖 13**： 在初始頁面負載不會顯示產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


因為"--選擇類別目錄-」 清單項目被選取時顯示任何產品的原因是因為它的值是`-1`並在資料庫中，沒有產品`CategoryID`的`-1`。 如果這是您想在此時完成時的行為 ！ 不過，如果您想要顯示*所有*一個類別目錄選取的 「-選擇類別目錄-」 清單項目時，返回`ProductsBLL`類別，並自訂`GetProductsByCategoryID(categoryID)`方法，讓它叫用`GetProducts()`方法如果傳遞中*`categoryID`* 參數小於零：


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

此處所使用的技巧是類似於我們用來顯示所有的供應商方法回到[宣告式的參數](../basic-reporting/declarative-parameters-cs.md)教學課程中，雖然此範例中我們使用值為`-1`表示應該所有記錄擷取相對於`Nothing`。 這是因為*`categoryID`* 參數`GetProductsByCategoryID(categoryID)`方法應為整數值通過，而在宣告式參數教學課程中我們所傳入的字串輸入參數。

[圖 14] 顯示的螢幕擷取畫面`FilterByDropDownList.aspx`時 「-選擇類別目錄-」 選項。 這裡的所有產品依預設，會顯示，而使用者可以選擇特定類別目錄來縮小顯示。


[![所有產品都是現在列出預設](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**圖 14**： 所有產品是現在列 By Default ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>總結

顯示階層關聯的資料時，通常最好來呈現使用主版/詳細報告，使用者可以從中開始研究過的資料階層的頂端，並向下鑽研詳細資料的資料。 在本教學課程中，我們檢查建置簡單的主要/詳細資料報表，顯示所選取之目錄的產品。 這被透過使用 dropdownlist 進行分類和產品屬於所選分類 GridView 的清單。

在 [下一個教學課程](master-detail-filtering-with-two-dropdownlists-vb.md)我們將 DropDownList 介面進一步採取步驟，使用兩個 dropdownlist 進行。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [下一頁](master-detail-filtering-with-two-dropdownlists-vb.md)
