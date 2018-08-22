---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: 新增 GridView 資料行的核取方塊 (VB) |Microsoft Docs
author: rick-anderson
description: 本教學課程會探討如何將為使用者提供以直覺的方式，來選取多個資料列的 G.的 GridView 控制項中的核取方塊的資料行...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: be7fc4fe93d15e7550f873adffa15e82384d25be
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830778"
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>新增 GridView 資料行的核取方塊 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe)或[下載 PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> 本教學課程會探討如何將為使用者提供以直覺的方式，來選取多個資料列的 GridView 的 GridView 控制項中的核取方塊的資料行。


## <a name="introduction"></a>簡介

在先前的教學課程中，我們會檢查如何將選項按鈕的資料行加入至 GridView，以選取特定的記錄。 當使用者選擇最多一個項目，從方格中有限時，選項按鈕的資料行是合適的使用者介面。 有些時候，不過，我們可能會想要讓使用者能夠選取任意數目的方格中的項目。 比方說，web 型電子郵件用戶端，通常顯示核取方塊的資料行的訊息的清單。 使用者可以選取任意數目的訊息，並接著執行某些動作，例如將電子郵件移到另一個資料夾，或刪除它們。

在本教學課程中，我們會看到如何將資料行的核取方塊以及如何判斷在回傳時簽入哪些核取方塊。 特別是，我們將建置的範例，精確地模擬 web 型電子郵件用戶端使用者介面。 我們的範例中會包含清單中的產品分頁的 GridView`Products`資料庫資料表的核取方塊，在每個資料列 （請參閱 圖 1）。 刪除選取的產品按鈕，按一下時，將會刪除選取的產品。


[![每個產品的資料列包含一個核取方塊](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**圖 1**： 每個產品的資料列包含一個核取方塊 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>步驟 1： 加入分頁的 GridView，列出產品資訊

我們會擔心加入資料行的核取方塊之前，讓 s 第一個著重於清單中的 GridView 會支援分頁的產品。 首先開啟`CheckBoxField.aspx`頁面中`EnhancedGridView`資料夾，然後拖曳的 GridView，從 [工具箱] 拖曳至設計工具，設定其`ID`至`Products`。 接下來，選擇要繫結至名為新 ObjectDataSource 的 GridView `ProductsDataSource`。 設定要使用 ObjectDataSource`ProductsBLL`類別，呼叫`GetProducts()`方法傳回的資料。 此 GridView 會處於唯讀模式，因為設定下拉式清單中更新、 插入和刪除索引標籤為 （無）。


[![建立名為 ProductsDataSource 新 ObjectDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**圖 2**： 建立新的 ObjectDataSource 具名`ProductsDataSource`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![設定要使用 GetProducts() 方法擷取資料的 ObjectDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**圖 3**： 設定要擷取的資料使用 ObjectDataSource`GetProducts()`方法 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**圖 4**： 設定下拉式清單中更新、 插入和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


完成設定資料來源精靈之後，Visual Studio 會自動建立 BoundColumns 和 CheckBoxColumn 產品相關的資料欄位。 像我們在上一個教學課程中，移除以外的所有`ProductName`， `CategoryName`，並`UnitPrice`BoundFields，並將變更`HeaderText`產品、 類別和價格的屬性。 設定`UnitPrice`BoundField，讓其值會格式化為貨幣。 也設定 GridView，以支援分頁，藉由檢查智慧標籤啟用分頁 核取方塊。

可讓 s 也將刪除所選的產品的使用者介面。 加入 Button Web 控制項下方 GridView，設定其`ID`要`DeleteSelectedProducts`及其`Text`屬性，以刪除選取的產品。 而不是實際刪除資料庫中的產品，此範例中我們只想顯示訊息，指出產品會被刪除。 若要做到這一點，新增至按鈕下方的標籤 Web 控制項。 將其識別碼設`DeleteResults`，請參閱清除其`Text`屬性，並設定其`Visible`並`EnableViewState`屬性，以`False`。

進行這些變更之後，GridView、 ObjectDataSource、 按鈕和標籤 s 宣告式標記應該如下所示：


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

請花一點時間瀏覽器中檢視頁面 （請參閱 [圖 5]）。 此時您應該會看到名稱、 類別和的前十個產品的價格。


[![列出名稱、 類別和第十個產品的價格](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**圖 5**： 列出 Name、 Category 和第十個產品的價格 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>步驟 2： 加入資料行的核取方塊

由於 ASP.NET 2.0 包含 CheckBoxField，有人可能會認為它可以用來將資料行的核取方塊新增至 GridView。 不幸的是，不在案例中，如 CheckBoxField 設計來搭配布林值的資料欄位。 亦即，若要使用 CheckBoxField 我們必須指定其值會參考要判斷是否要檢查的呈現核取方塊的基礎資料欄位。 我們不能使用 CheckBoxField 只包含資料行的核取核取方塊。

相反地，我們必須在 新增為 TemplateField，然後將核取方塊 Web 控制項，加入其`ItemTemplate`。 繼續並加入至為 TemplateField `Products` GridView，並使它的第一個 （最左邊） 欄位。 從 GridView s 智慧標籤，按一下 [編輯範本] 連結，然後拖曳核取方塊 Web 控制項從工具箱拖曳到`ItemTemplate`。 設定此核取方塊 s`ID`屬性設`ProductSelector`。


[![新增名為 TemplateField 的 ItemTemplate 的 ProductSelector 的核取方塊 Web 控制項](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**圖 6**： 新增核取方塊 Web 控制項名為`ProductSelector`TemplateField s `ItemTemplate` ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


與新增 TemplateField 和核取方塊 Web 控制項，每個資料列現在包含一個核取方塊。 圖 7 顯示此頁面上，新增 TemplateField 和核取方塊之後，在瀏覽器檢視時。


[![每個產品的資料列現在包含一個核取方塊](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**圖 7**： 每個產品的資料列現在包含一個核取方塊 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>步驟 3： 決定哪些核取方塊在回傳時簽入

此時，我們會有核取方塊，但沒有辦法決定何種核取方塊已核取回傳的資料行。 按一下 [刪除選取的產品] 按鈕時，不過，我們需要知道哪些核取方塊已核取若要刪除這些產品。

GridView s [ `Rows`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx)提供 GridView 中資料列的存取。 我們可以逐一查看這些資料列，以程式設計方式存取 核取方塊控制項，並請參考其`Checked`屬性來判斷是否已選取此核取方塊。

建立事件處理常式`DeleteSelectedProducts`Button Web 控制項的`Click`事件，並新增下列程式碼：


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

`Rows`屬性傳回的集合`GridViewRow`GridView 的資料列執行個體的結構。 `For Each`這裡迴圈會列舉此集合。 每個`GridViewRow`物件，使用以程式設計方式存取資料列的 [s] 核取方塊`row.FindControl("controlID")`。 如果勾選此核取方塊，則對應的資料列 s`ProductID`值從擷取`DataKeys`集合。 在此練習中，我們只是顯示在資訊訊息`DeleteResults`加上標籤，雖然在運作中應用程式 d 我們改為呼叫`ProductsBLL`類別的`DeleteProduct(productID)`方法。

加入此事件處理常式之後，按一下 [刪除選取的產品] 按鈕現在會顯示`ProductID`s 所選的產品。


[![按一下 刪除選取的產品按鈕時即會顯示選取的產品 Productid](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**圖 8**： 當刪除選取的產品按鈕選取的產品`ProductID`橋接器列示 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>步驟 4： 新增核取所有，並取消選取所有按鈕

如果使用者想要刪除目前的頁面上的所有產品，它們必須檢查每十個核取方塊。 我們可以協助加速此程序加上核取所有按鈕，按一下時，在方格中選取所有核取方塊。 取消核取 [全部] 按鈕會同樣很有幫助。

兩個按鈕 Web 將控制項加入至頁面上，將它們放入上述 GridView。 設定第一個 s`ID`來`CheckAll`並將其`Text`屬性來檢查所有; 集合的第二項 s`ID`來`UncheckAll`並將其`Text`來取消選取所有的屬性。


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

接下來，建立名為的程式碼後置類別中的 方法`ToggleCheckState(checkState)`，叫用時，會列舉`Products`GridView s`Rows`集合，並設定每個核取方塊 s`Checked`屬性設為傳遞的值在*已核取 checkState*參數。


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

接下來，建立`Click`事件處理常式`CheckAll`和`UncheckAll`按鈕。 在  `CheckAll` s 事件處理常式，只需呼叫`ToggleCheckState(True)`; 在`UncheckAll`，呼叫`ToggleCheckState(False)`。


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

使用此程式碼中，按一下 [查看全部] 按鈕造成回傳，並檢查所有在 GridView 的核取方塊。 同樣地，按一下 取消選取全部取消選取所有核取方塊。 圖 9 顯示的畫面之後已核取 查看全部 按鈕。


[![按一下核取所有按鈕會選取所有核取方塊](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**圖 9**： 按一下核取所有按鈕會選取所有核取方塊 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> 當顯示的資料行的核取方塊，選取或取消選取所有核取方塊的其中一種方法是透過標頭資料列中的核取方塊。 此外，目前全選/取消核取所有的實作所需的回傳。 核取方塊已核取或取消核取，但是，可能會完全是透過用戶端指令碼，進而提供更快速得到的使用者體驗。 若要瀏覽詳細資料，以及使用用戶端技術的討論中，使用的所有核取及取消核取所有的標頭資料列核取方塊中，請參閱[檢查所有核取方塊在 GridView 使用用戶端指令碼，並選取所有核取方塊](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx)。


## <a name="summary"></a>總結

在您要讓使用者從一個 GridView，再繼續選擇任意數目的資料列的情況下，加入資料行的核取方塊是選項之一。 如我們所見本教學課程中，包括在 GridView 的核取方塊的資料行，必須新增為 TemplateField 與核取方塊 Web 控制項。 使用 Web 控制項 （相對於如同我們在上一個教學課程，請直接在範本中，插入標記） 項目核取方塊已和不簽入跨越回傳，自動會記住 ASP.NET。 我們也以程式設計的方式可以存取的核取方塊中的程式碼來判斷是否已使用指定的核取方塊，或變更核取的狀態。

本教學課程和最後一個討論過的資料列選取器的資料行加入至 GridView。 在我們的下一個教學課程中，我們將檢驗如何，一些工作，我們可以新增插入功能至 GridView。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](adding-a-gridview-column-of-radio-buttons-vb.md)
> [下一頁](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
