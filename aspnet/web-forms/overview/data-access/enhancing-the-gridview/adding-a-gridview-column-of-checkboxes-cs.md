---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: 加入 GridView 的資料行的核取方塊 (C#) |Microsoft 文件
author: rick-anderson
description: 本教學課程會查看如何核取方塊的資料行加入提供給使用者以直覺的方式，選取代表 g 的多個資料列的 GridView 控制項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c9ad4c66c5bdd24df691180acf354f666e6e578
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880163"
---
<a name="adding-a-gridview-column-of-checkboxes-c"></a>加入 GridView 的資料行的核取方塊 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe)或[下載 PDF](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> 本教學課程會查看如何提供給使用者以直覺的方式選取多個資料列的 GridView 的 GridView 控制項中加入資料行的核取方塊。


## <a name="introduction"></a>簡介

在前述教學課程中我們會檢查如何加入資料行的選項按鈕，以選取特定資料錄 GridView。 當使用者選擇最多一個項目，從方格中有限時，選項按鈕的資料行是合適的使用者介面。 有時候，不過，我們可能想要讓使用者能夠選取任意數目的方格中的項目。 例如，網頁型電子郵件用戶端，通常顯示具有資料行的核取方塊的訊息清單。 使用者可以選取任意數目的訊息，並接著執行某些動作，例如，電子郵件移到其他資料夾，或刪除它們。

在本教學課程中，我們會看到如何將資料行的核取方塊以及如何判斷在回傳簽入哪些核取方塊。 特別是，我們會建置慎網頁型電子郵件用戶端使用者介面的範例。 我們的範例中會包含以列出產品的分頁的 GridView`Products`資料庫資料表的核取方塊，在每個資料列 （請參閱圖 1）。 刪除選取的產品按鈕，按一下時，將會刪除選取的產品。


[![每個產品的資料列包含核取方塊](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**圖 1**： 每個產品的資料列包含核取方塊 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>步驟 1： 加入分頁的 GridView 會列出產品資訊

我們會擔心加入資料行的核取方塊之前，可讓 s 以列出產品的 GridView 會支援分頁上的第一個焦點。 先開啟`CheckBoxField.aspx`頁面`EnhancedGridView`資料夾，然後拖曳 GridView 從工具箱拖曳至設計工具中，設定其`ID`至`Products`。 接下來，選擇將 GridView 繫結至名為新 ObjectDataSource `ProductsDataSource`。 設定用於 ObjectDataSource`ProductsBLL`類別，呼叫`GetProducts()`方法傳回的資料。 此 GridView 將處於唯讀模式，因為設定下拉式清單中更新、 插入和刪除索引標籤為 （無）。


[![建立名為 ProductsDataSource 新 ObjectDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**圖 2**： 建立新的 ObjectDataSource 具名`ProductsDataSource`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))


[![設定要擷取資料使用 GetProducts() 方法 ObjectDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**圖 3**： 設定要擷取的資料使用 ObjectDataSource`GetProducts()`方法 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**圖 4**： 下拉式清單中設定更新、 插入和刪除的索引標籤 （無） ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))


完成設定資料來源精靈之後，Visual Studio 會自動建立 BoundColumns 和 CheckBoxColumn 產品相關的資料欄位。 我們未在上一個教學課程中的類似，請移除以外的所有`ProductName`， `CategoryName`，和`UnitPrice`BoundFields，並變更`HeaderText`產品、 類別和價格的屬性。 設定`UnitPrice`BoundField，使其值的格式為貨幣。 也請設定以支援分頁，方式是檢查智慧標籤啟用分頁 核取方塊 GridView。

可讓 s 也將刪除所選的產品的使用者介面。 加入按鈕 Web 控制項下方 GridView 中設定其`ID`至`DeleteSelectedProducts`及其`Text`屬性，以刪除選取的產品。 而不實際刪除資料庫中的產品，此範例中我們將只會顯示訊息，指出會被刪除的產品。 若要做到這一點，加入至按鈕下方的標籤 Web 控制項。 將其識別碼設`DeleteResults`，清除出其`Text`屬性，並設定其`Visible`和`EnableViewState`屬性`false`。

進行這些變更之後，GridView、 ObjectDataSource、 按鈕和標籤 s 宣告式標記應該如下所示：


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

請花一點時間瀏覽器中檢視頁面 （請參閱圖 5）。 此時您應該會看到名稱、 類別和的前十個產品的價格。


[![會列出名稱、 類別和第十個產品的價格](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**圖 5**： 列出名稱、 類別和第十個產品的價格 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>步驟 2： 加入資料行的核取方塊

因為 ASP.NET 2.0 包含 CheckBoxField，其中可能會認為它無法使用加入的 GridView 的資料行的核取方塊。 不幸的是，不情況下，因為將 CheckBoxField 設計來搭配布林值的資料欄位。 也就是說，才能使用將 CheckBoxField 我們必須指定其值查閱來決定是否要檢查呈現的核取方塊的基礎資料欄位。 我們無法使用 CheckBoxField 只包含未選取核取方塊的資料行。

相反地，我們必須加入為 TemplateField，將核取方塊 Web 控制項加入其`ItemTemplate`。 請繼續並加入至 TemplateField `Products` GridView，並將其第一個 （最左邊） 欄位。 從 GridView s 智慧標籤，按一下 [編輯樣板] 連結，然後拖曳核取方塊 Web 控制項從工具箱拖曳到`ItemTemplate`。 設定此核取方塊 s`ID`屬性`ProductSelector`。


[![加入名為 TemplateField 的 ItemTemplate 的 ProductSelector 核取方塊 Web 控制項](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**圖 6**： 加入核取方塊 Web 控制項名為`ProductSelector`TemplateField s `ItemTemplate` ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))


與加入 TemplateField 和核取方塊 Web 控制項，每個資料列現在包含核取方塊。 圖 7 顯示此頁面上，新增 TemplateField 和核取方塊之後，透過瀏覽器中，檢視時。


[![每個產品的資料列現在包含核取方塊](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**圖 7**： 現在包含每個產品的資料列的核取方塊 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>步驟 3： 決定哪些核取方塊在回傳簽入

現在我們有核取方塊，但沒有方法可以判斷哪些核取方塊在回傳簽入的資料行。 按一下 [刪除選取的產品] 按鈕時，不過，我們需要知道哪些核取方塊已接受檢查，以刪除這些產品。

GridView s [ `Rows`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx)提供在 GridView 的資料列的存取。 我們可以逐一查看這些資料列，以程式設計方式存取 [CheckBox] 控制項，然後再參閱其`Checked`屬性來判斷是否已選取核取方塊。

建立事件處理常式`DeleteSelectedProducts`按鈕 Web 控制項的`Click`事件並加入下列程式碼：


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

`Rows`屬性傳回的集合`GridViewRow`GridView 的資料列執行個體的架構。 `foreach`這裡迴圈列舉此集合。 每個`GridViewRow`物件，使用以程式設計方式存取資料列 s 核取方塊`row.FindControl("controlID")`。 如果勾選此核取方塊，則對應的資料列 s`ProductID`值從擷取`DataKeys`集合。 在此練習中，我們只是顯示在資訊訊息`DeleteResults`加上標籤，雖然可用的應用程式中，我們 d 反而是讓呼叫`ProductsBLL`類別的`DeleteProduct(productID)`方法。

加入此事件處理常式之後，按一下 [刪除選取的產品] 按鈕現在會顯示`ProductID`之選取的產品。


[![刪除選取的產品按鈕便會列出選取產品 Productid](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**圖 8**： 時刪除選取的產品按鈕選取的產品`ProductID`橋接器列示 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>步驟 4： 加入核取所有，並取消選取所有按鈕

如果使用者想要刪除目前的頁面上的所有產品，它們必須檢查每十個核取方塊。 我們可以協助加速此程序將檢查所有按鈕，按一下時，在方格中選取所有核取方塊。 取消核取所有的按鈕將會同樣很有用。

將兩個按鈕 Web 控制項加入至頁面上，將它們全放置在 GridView 上方。 設定第一個 s`ID`至`CheckAll`及其`Text`屬性來檢查所有; 設定第二個一個 s`ID`來`UncheckAll`及其`Text`要取消選取所有屬性。


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

接下來，建立名為的程式碼後置類別中的 方法`ToggleCheckState(checkState)`，叫用時，會列舉`Products`GridView s`Rows`集合，並設定每個核取方塊 s`Checked`屬性設為值的傳入中*checkState*參數。


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

接下來，建立`Click`事件處理常式`CheckAll`和`UncheckAll`按鈕。 在`CheckAll`s 事件處理常式，只需呼叫`ToggleCheckState(true)`; 在`UncheckAll`，呼叫`ToggleCheckState(false)`。


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

使用此程式碼中，按一下核取所有的按鈕導致回傳，並檢查所有在 GridView 的核取方塊。 同樣地，按一下 取消選取全部取消選取所有核取方塊。 圖 9 顯示螢幕之後已核取核取所有的按鈕。


[![按一下核取所有按鈕會選取所有核取方塊](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**圖 9**： 按一下核取所有按鈕選取所有核取方塊 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))


> [!NOTE]
> 當顯示的資料行的核取方塊，選取或取消選取所有核取方塊的其中一個方法是透過標頭列中的核取方塊。 此外，目前核取所有/取消選取所有實作都必須回傳。 核取方塊可以選取或取消選取，不過，會完全透過用戶端指令碼，進而提供較 snappier 的使用者體驗。 若要瀏覽詳細資料，以及使用用戶端技術的討論中，使用的所有核取及取消核取所有的標頭資料列核取方塊取出[檢查所有核取方塊在 GridView 使用用戶端指令碼，並選取所有核取方塊](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx)。


## <a name="summary"></a>總結

在您要讓使用者選擇從之前的 GridView 的任意數目的資料列的情況下，加入資料行的核取方塊是其中一個選項。 如我們所見本教學課程中，核取方塊在 GridView 的資料行包括牽涉到新增為 TemplateField 與核取方塊 Web 控制項。 使用 Web 控制項 （相對於如我們所做的上一個教學課程中，請直接將此範本中，插入標記） ASP.NET 自動會記住什麼核取方塊已和未檢查跨回傳。 我們也以程式設計方式可存取的核取方塊中的程式碼來判斷是否會檢查指定的核取方塊，或變更的核取的狀態。

在 GridView 中加入資料列選取器的資料行，查看本教學課程和最後一個。 在我們的下一個教學課程中，我們將檢驗如何進行一些工作，我們可以新增插入功能至 GridView。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](adding-a-gridview-column-of-radio-buttons-cs.md)
> [下一頁](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
